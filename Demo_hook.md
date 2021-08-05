
## Cách một: Hook vào update_node_form để thêm một số tính năng
```php
function demo_hook_form_alter(&$form, &$form_state, $form_id) {
//Hook $form_id để chọn duy nhất mọt form.
vd:  if ($form_id === 'update_node_form') {
        $form['demo1'] = [
        '#type' => 'textfield',
        '#title' => t('Tên'),
        '#size' => 14,
        '#maxlength' => 4,
        '#weight' => -10,
        ];

//Hook vào submit để đưa hàm lên đầu, ta dùng lệnh '#weight'.
vd:    $form['submit']['#weight'] = -15;

//Hook vào content_types để thay đôi #title.
vd:    $form['content_types']['#title'] = t('Chọn content type');

//Hook vào update để thay đổi #title.
vd:    $form['update']['#title'] = t('Nhập  nội dung');
  }
}
```


## Cách 2: Thay vì gọi hàn if riêng như cách một thì C2 mình add vào function luôn.
```php
//Cách này mik hook thẳng vào function để gọi tới form.
vd:    function demo_hook_form_update_node_form_alter(&$form, &$form_state, $form_id) {
    $form['demo1'] = [
        '#type' => 'textfield',
        '#title' => t('test'),
        '#size' => 20,
        '#maxlength' => 40,
        '#weight' => -10,
    ];

//Hook vào submit để đưa hàm lên đầu, ta dùng lệnh '#weight'.
vd:    $form['submit']['#weight'] = -15;

//Hook vào content_types để thay đôi #title.
vd:    $form['content_types']['#title'] = t('Chọn content type');

//Hook vào update để thay đổi #title.
vd:    $form['update']['#title'] = t('Nhập  nội dung');
}
```
