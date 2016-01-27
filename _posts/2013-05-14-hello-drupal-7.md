---
layout: post
title: Hello Drupal 7!
created: 1368564902
comments: true
categories: [drupal]
---
I have moved my Drupal 6 blog into Drupal 7 and migrated all the content including static pages, blog posts and images. For the migration I used the <a href="http://drupal.org/project/migrate">migrate</a> module. I found it very powerful.

Also as a big fan of Twitter Bootstrap, the template was based on it making the mobile as the primary platform. The idea on the main page is that I can replace the background image with my latest best photography. I'm not an expert, but sometimes my photos are very scenic.

The migrate module is designed to migrate content into Drupal from various sources and has Oracle, MsSQL, CSV connectors as well as Drupal 6 with nice examples. The best feature is the dependency management. If an entity is referencing to an other entity, then I can define this dependency in the migration class and the module will handle it.

In order to import the categories, I need to register the migration with a hook, like mymodule_register_migrations. Replace the mymodule with the proper name of your custom module.

<code class="php">
  $common_arguments = array(
    'source_connection' => 'legacy',
    'source_version' => 6,
  );
  $vocabulary_arguments = array(
    array(
      'description' => t('Migration of Blog term'),
      'machine_name' => 'Tags',
      'source_vocabulary' => '1',  // D6 Vocabulary ID
      'destination_vocabulary' => 'tags',
    ),
  );
  $common_vocabulary_arguments = $common_arguments + array(
    'class_name' => 'DrupalTerm6Migration',
    'soft_dependencies' => array(),
  );
  foreach ($vocabulary_arguments as $arguments) {
    $arguments += $common_vocabulary_arguments;
    Migration::registerMigration($arguments['class_name'], $arguments['machine_name'],
                                 $arguments);
  }
</code>

I will reuse $common_arguments, so the effective code is much smaller for the whole registration.

In order to import image fields from Drupal 6, there are many ways to success, I choose to import the images first, then make a dependency on the images from node fields rather than import the images with the nodes.

<code class="php">
  $image_arguments = array(
    array(
      'class_name' => 'SandorImageMigration',
      'description' => t('Migration of images from Drupal 6'),
      'machine_name' => 'Image',
      'source_type' => 'field_leftgallery',
      'destination_type' => 'field_leftgallery',
    ),
    array(
      'class_name' => 'SandorScreenshotMigration',
      'description' => t('Migration of screenshots from Drupal 6'),
      'machine_name' => 'Screenshot',
      'source_type' => 'field_portfolioimage',
      'destination_type' => 'field_portfolio_image',
    ),
  );
  $common_image_arguments = $common_arguments + array(
    'user_migration' => 'User',
  );
  foreach ($image_arguments as $arguments) {
    $arguments = array_merge_recursive($arguments, $common_image_arguments);
    Migration::registerMigration($arguments['class_name'], $arguments['machine_name'],
                                 $arguments);
  }
</code>

And so on, I'm not going to share the whole source code, but the documentation is really good and the source code of the module is very handy. As you can see, I have imported the two field separatedly. <a href="http://drupal.org/project/migrate_d2d">migrate_d2d</a> by default is trying to import everything and for such reason, I had to extend the SQL query generator to filter the field results to only one image field. This can be done easily:

<code class="php">
/**
 * Image-specific mappings and handling.
 */
class SandorImageMigration extends DrupalFile6Migration {
  public function __construct(array $arguments) {
    parent::__construct($arguments);
    $this->removeFieldMapping('source_dir');
    $this->addFieldMapping('source_dir')
         ->defaultValue('/home/zoner/Development/http/sandorfiles/');
  }

  protected function query() {
    // Get the default query and modify it
    $query = parent::query();
    $query->join('content_field_leftgallery', 'ti', 'f.fid = ti.field_leftgallery_fid');
    return $query;
  }
}
</code>

Because of this, the node import is going to be more tricky, but I saved some development time on importing the images once. Let's see the blogpost import class:

<code class="php">
class SandorArticleMigration extends SandorNodeMigration {

  public function prepareRow($row) {
    parent::prepareRow($row);

    $query = Database::getConnection('default', $this->sourceConnection)
             ->select('term_node', 'tn')
             ->fields('tn', array('tid'))
             ->fields('td', array('name'))
             ->condition('tn.nid', $row->nid);
    $query->join('term_data', 'td', 'td.tid = tn.tid');
    $query->fields('td', array('vid'));
    $result = $query->execute();
    $terms = array();
    foreach ($result as $term_row) {
      $terms[] = $term_row->name;
    }
    $row->taxonomy_vocab_1 = implode(',', $terms);
  }

  public function __construct(array $arguments) {

    parent::__construct($arguments);

    $this->addFieldMapping('field_leftgallery', 'field_leftgallery')
         ->sourceMigration('Image');
    $this->addFieldMapping('field_leftgallery:file_class')->defaultValue('MigrateFileFid');
    // Note that we map migrated terms by the vocabulary ID.
    $this->addFieldMapping('field_tags', 'taxonomy_vocab_1')
         ->separator(',');
  }
}
</code>

The trick is that I need to tell the field it will import a FileFid instead of a FileName and also, there are some more logic to fill in the Tags field with a comma separated value, extracted from the Drupal 6 tables. Remember, I have already created the Terms as the blogposts are depending from them.
