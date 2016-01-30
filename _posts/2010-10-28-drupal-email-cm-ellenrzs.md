---
layout: post
title: Drupal email cím ellenőrzés
created: 1288256858
comments: true
---
Ha Form API-val hoztuk létre a Drupal formot, használhatjuk a Drupal beépített email ellenőrzését validáció közben így:

<php>
/**
 * Validate form
 */
function modulneve_formneve_validate($form, &$form_state) {
  if(!valid_email_address($form_state['values']['email_address'])) {
    form_set_error('email_address', t('Please enter a valid email address'));
  }
}
</php>
