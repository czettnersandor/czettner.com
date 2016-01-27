---
layout: post
title: Source code typography in PHP
created: 1369076424
comments: true
categories: [php]
---
From the project manager's point of view, the developer is that kind of animal who can make things and also fix things. Everybody thinks the developer writes code, typing magical words into a typewriter like text editor which automatically changes it's colour without selecting the text and pressing any colour icon on the toolbar. They can see how we work. But they can't see how we spend our rest of the work hours with the source code because effectively there's nothing to see. Because we read the code, we read more code than we type.

This is why the beauty of the code is so important. It's not just because we like to see the syntax highlighted version of our implemented ideas, but a well written code saves a lot of time when a new developer comes and needs to understand what we did. Or even for us, we should read our own code a year later and if this code is not readable, then we will blame the author, even if we should blame ourselves.

Because of this, source code should be written to be understood by real people rather than written for the computer to parse faster or run faster. If the source code should be modified in order to run it radically faster, then the basic idea was probably wrong. Even if the code runs incredibly fast, but it's impossible to read and modify, it was a wrong implementation. It would be cheaper to buy more RAM and server instance rather than buy valuable work-hours to maintain this code.

In my daily job, I'm working with PHP, usually writing Magento or Drupal code. Both has a very strict coding standard which is recommended to follow. In this way, the whole project has a very comfortable feeling when work with. The feeling that tells me it's a solid system, it was written by following high standards and there was no contributor in the team who violated the standards. I feel harmony.

Typography in the basic manner means where does the letter look like and where it go. So this word is easily applicable for programming. In PHP, there is no one way. We can go the C way, we can go the ruby way, but because of the popularity of the language the typography variations are very different. And there are so many. Way much as it should be. PHP is not Python or Coffeescript where the mistyped indentation or new line can easily generate fatal error or makes seriously different behaviour. PHP was designed for the crowd, it's very easy to write rubbish code in PHP. But for god's sake, please don't.

Is indenting and commenting our code enough? Let's find out.

<code class="php">
class Mage_Adminhtml_Block_Api_User_Edit_Tab_Roles extends Mage_Adminhtml_Block_Widget_Grid
{
    protected function _prepareColumns()
    {

        $this->addColumn('assigned_user_role', array(
            'header_css_class' => 'a-center',
            'header'    => Mage::helper('adminhtml')->__('Assigned'),
            'type'      => 'radio',
            'html_name' => 'roles[]',
            'values'    => $this->_getSelectedRoles(),
            'align'     => 'center',
            'index'     => 'role_id'
        ));
</code>

I like how the array hooks together. It is good for the eyes in this example. The eye flows easily over it and it's clear where is the key and where is the value. And because of the double closing parenthesis it's easy to understand we closed the function call as well.

<code class="php">
class Mage_GiftMessage_Block_Message_Form extends Mage_Core_Block_Template
{

    public function getSaveUrl()
    {
        return $this->helper('giftmessage/url')->getSaveUrl(
                            $this->getRequest()->getParam('item'),
                            $this->getRequest()->getParam('type'),
                            $this->getRequest()->getParam('message'),
                            array('uniqueId'=>$this->getRequest()->getParam('uniqueId'))
        );
    }
</code>

This is where the language limitations taking place. It's clearly not good as this can happen again in the code and makes the indentation fuzzy with virtually different deepness of the parameter list because the length of the function name can be different. I would rather see a single indentation instead. It would improve the readability so much.

The <a href="http://www.php-fig.org/">Framework Interop Group</a> has proposed and approved a series of style recommendations, known as <a href="https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md">PSR-0</a>, <a href="https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md">PSR-1</a> and <a href="https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md">PSR-2</a>. Don’t let the funny names confuse you, these recommendations are merely a set of rules that Drupal, Zend, Symfony are starting to adopt. You can use them for your own projects, or continue to use your own personal style.

The <a href="http://pear.php.net/package/PHP_CodeSniffer/">PHP Code Sniffer</a> is use to validate messy or non standard code and in my experience it is very helpful if your text editor/IDE is configured to use the PHP Code Sniffer. It tokenises PHP, JavaScript and CSS files and detects violations of a defined set of coding standards. In my favourite text editor, Sublime Text the <a href="https://github.com/benmatselby/sublime-phpcs">Phpcs package</a> can highlight the faulty lines while editing.
