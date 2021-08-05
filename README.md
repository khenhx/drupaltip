UpdateNodeForm
```php
public function submitForm(array &$form, FormStateInterface $form_state) {
    $contentType = $form_state->getValue('content_types');
    $storage_handler = \Drupal::entityTypeManager()->getStorage("node");
    $entities = $storage_handler->loadByProperties(["type" => $contentType]);
    if (count($entities) === 0) {
      \Drupal::messenger()->addMessage('Ko có node để Update');
    }
    else {
      $update = $form_state->getValue('update');
      foreach ($entities as $node) {
        $newTitle = $update .' '. $node->label();
        $node->set('title', $newTitle);
        $node->save();
      }
      \Drupal::messenger()->addMessage('Update thành công : ' . count($entities) . ' node.');
}
```
