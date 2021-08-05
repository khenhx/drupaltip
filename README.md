## UpdateNodeForm:
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



## CreateNodeForm:
```php
  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $tongNode = $form_state->getValue('tong_node');
    $this->createUser($tongNode, $form_state);
    \Drupal::messenger()->addMessage('Tạo thành công : ' . $tongNode . ' node.');
  }

  private function createUser($sum, FormStateInterface $form_state) {
    $name = $form_state->getValue('name');
    $aos = $form_state->getValue('aos');
    for ($i = 0; $i < $sum; $i++) {
      $my_article = Node::create(['type' => 'Product']);
      $my_article->set('title', $name . $i);
      $my_article->set('body', 'My text vgfjghj' . $i);
      $my_article->set('field_may', $aos . $i);

      $now = DrupalDateTime::createFromTimestamp(time());
      $now->setTimezone(new \DateTimeZone('Europe/Belgrade'));
      $my_article->set('field_ngay_nhap_hang', $now->format('Y-m-d'));
      $my_article->enforceIsNew();
      $my_article->save();
    }
  }
```

## ContentTypeForm:
```php
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $tongNode = $form_state->getValue('tong_node');
    $this->createUser($tongNode, $form_state);
    \Drupal::messenger()->addMessage('Tạo thành công : ' . $tongNode . ' node.');
  }

  private function createUser($sum, $form_state) {
    $name = $form_state->getValue('name');
    $contentType = $form_state->getValue('content_type');
    for ($i = 0; $i < $sum; $i++) {
      $my_article = Node::create(['type' => $contentType]);
      $my_article->set('title', $name . $i);
      $my_article->enforceIsNew();
      $my_article->save();
    }
  }
```


## CreateUserByRoleForm:
```php
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $tongUser = $form_state->getValue('tong_user');
    $role = $form_state->getValue('role');

    if ($role === 'anonymous') {
      \Drupal::messenger()->addMessage('ko tạo được user với role anonymous');
    }
    else {
      $this->createUser($tongUser, $role);
      \Drupal::messenger()->addMessage('Tạo thành công : ' . $tongUser . ' user với role ' . $role);
    }
  }

  private function createUser($tongUser, $role) {
    for ($i = 0; $i < $tongUser; $i++) {
      // Create user object.
      $user = User::create();
      $user->setPassword("demo");
      $user->enforceIsNew();
      $user->setEmail("demo" . $i . "@gmail.com");
      $user->setUsername("demo" . $i . time());
      $user->addRole($role);
      $user->activate();
      $user->save();
    }
  }
```


## CreateUserForm:
```php
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $tongUser = $form_state->getValue('tong_user');
    $this->createUser($tongUser);
    \Drupal::messenger()->addMessage('Tạo thành công : ' . $tongUser . ' user.');
  }

  private function createUser($tongUser) {
    for ($i = 0; $i < $tongUser; $i++) {
      // Create user object.
      $user = User::create();
      $user->setPassword("demo");
      $user->enforceIsNew();
      $user->setEmail("demo" . $i . "@gmail.com");
      $user->setUsername("demo" . $i);
      $user->activate();
      $user->save();
    }
  }
```


## DeleteMuitiNodeForm:
```php
 public function submitForm(array &$form, FormStateInterface $form_state) {
    $contentType = $form_state->getValue('content_types');
    $storage_handler = \Drupal::entityTypeManager()->getStorage("node");
    $entities = $storage_handler->loadByProperties(["type" => $contentType]);

    if (count($entities) === 0) {
      \Drupal::messenger()->addMessage('Ko có node để xóa');
    }
    else {
      $storage_handler->delete($entities);
      \Drupal::messenger()->addMessage('Xóa thành công : ' . count($entities) . ' node.');
    }
  }
```


## DeleteNodeForm:
```php
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $contentType = $form_state->getValue('content_type');
    $storage_handler = \Drupal::entityTypeManager()->getStorage("node");
    $entities = $storage_handler->loadByProperties(["type" => $contentType]);
    if (count($entities) === 0) {
      \Drupal::messenger()->addMessage('Ko có node để xóa');
    }
    else {
      $storage_handler->delete($entities);
      \Drupal::messenger()->addMessage('Xóa thành công : ' . count($entities) . ' node.');
    }
  }
```


## DeleteUserByRoleForm:
```php
  public function submitForm(array &$form, FormStateInterface $form_state) {
//    $role = $form_state->getValue('role');
//    $roles = $form_state->getValue('roles');
//
//    if ($role === 'anonymous') {
//      \Drupal::messenger()->addMessage('Ko có user để xóa với role anonymous');
//    }
//    else {
//      $storage_handler = \Drupal::entityTypeManager()->getStorage('user');
//      $uids = $storage_handler->getQuery()
//        ->condition('roles', array_filter($roles), 'IN')
//        ->execute();
//
//      $users = $storage_handler->loadMultiple($uids);
//      $storage_handler->delete($users);
//      \Drupal::messenger()->addMessage('xóa thành công : ' . count($uids) . ' user với role ' . $role);
//    }



    $roles = $form_state->getValue('roles');
    $storage_handler = \Drupal::entityTypeManager()->getStorage('user');
    $uids = $storage_handler->getQuery()
      ->condition('roles', array_filter($roles), 'IN')
      ->execute();

    $users = $storage_handler->loadMultiple($uids);
    $storage_handler->delete($users);
    \Drupal::messenger()->addMessage('xóa thành công : ' . count($uids) . ' user với role đã chọn');
  }
```


## DemoSettingsForm:
```php
  public function validateForm(array &$form, FormStateInterface $form_state) {
    // Hoàng Xuân khen
    $fullName = $form_state->getValue('full_name');

    $pattern = '/[0-9]+/';
    if (preg_match($pattern, $fullName)) {
      $form_state->setErrorByName('full_name', $this->t('Tên ko đc chưa số'));
    }

    $listName = explode(" ", $fullName);

    foreach ($listName as $value) {
      if ($value != ucfirst($value)) {
        $form_state->setErrorByName('full_name', $this->t('Tên phải viết hoa'));
      }
    }
    foreach ($listName as $value) {
      if ($value != ucfirst(strtolower($value))) {
        $form_state->setErrorByName('full_name', $this->t('Tên viết hoa sai chỗ'));
      }
    }

    if (strlen($form_state->getValue('first_name')) > 10) {
      $form_state->setErrorByName('first_name', $this->t('hO QUÁ DÀI'));
    }

    if (strlen($form_state->getValue('first_name')) <= 3) {
      $form_state->setErrorByName('first_name', $this->t('hO QUÁ NGẮN'));
    }

    if (strlen($form_state->getValue('last_name')) > 10) {
      $form_state->setErrorByName('last_name', $this->t('Tên QUÁ DÀI'));
    }

    if (strlen($form_state->getValue('last_name')) <= 3) {
      $form_state->setErrorByName('last_name', $this->t('Tên QUÁ NGẮN'));
    }

//    $birthdayInputYear = (int) substr($form_state->getValue('birthday'), 0, 4);
//    if ($birthdayInputYear < 1950) {
//      $form_state->setErrorByName('birthday', $this->t('Năm quá cũ'));
//    }
//
//    $currentYear = \Drupal::service('date.formatter')->format(\Drupal::time()->getCurrentTime(), 'custom', 'Y');
//
//    if ($birthdayInputYear > $currentYear) {
//      $form_state->setErrorByName('birthday', $this->t('Năm quá lớn'));
//    }

    $ngayTao = $form_state->getValue('ngay_tao')->format('Y');

    if ($ngayTao < 1950) {
      $form_state->setErrorByName('ngay_tao', $this->t('Năm quá cũ'));
    }
    $currentYear = \Drupal::service('date.formatter')->format(\Drupal::time()->getCurrentTime(), 'custom', 'Y');

    if ($ngayTao > $currentYear) {
      $form_state->setErrorByName('ngay_tao', $this->t('Năm quá lớn'));
    }
  }

  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state) {
    parent::submitForm($form, $form_state);

    $config = $this->config('demo.settings');

    $config
      ->set('full_name', $form_state->getValue('full_name'))
      ->set('last_name', $form_state->getValue('last_name'))
      ->set('first_name', $form_state->getValue('first_name'))
      ->set('birthday', $form_state->getValue('birthday'))
      ->set('ngay_tao', $form_state->getValue('ngay_tao')->getTimestamp())
      ->save();
  }
```


## EditSettingsForm:
```php
  public function submitForm(array &$form, FormStateInterface $form_state) {
    parent::submitForm($form, $form_state);

    $config = $this->config('edit.settings');

    $config
      ->set('full_name', $form_state->getValue('full_name'))
      ->set('last_name', $form_state->getValue('last_name'))
      ->set('first_name', $form_state->getValue('first_name'))
      ->save();
  }
```

## GroupSettingsForm:
```php
  public function submitForm(array &$form, FormStateInterface $form_state) {
    parent::submitForm($form, $form_state);

    $this->config('group.settings')
      ->set('first_name', $form_state->getValue('first_name'))
      ->set('last_name', $form_state->getValue('last_name'))
      ->set('full_name', $form_state->getValue('full_name'))
      ->set('pass', \Drupal::service('password')->hash($form_state->getValue('pass')))
      ->set('your_birthday', $form_state->getValue('your_birthday'))
      ->save();
  }
```
