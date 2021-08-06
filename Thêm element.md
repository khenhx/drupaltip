## Cấu trúc: Chuyển tất cả chữ hoa thành chữ thường, gắn giá trị vào elemant custom bằng cách dùng jquery để tương tác.
```javascript
vd:
(function($, Drupal) {
  'use strict';
  Drupal.behaviors.createNode = {
    attach: function(context) {
      function textthaydoi() {
        var value = $(this).val();
        var valueTemp = value.toLowerCase();
        $("#edit-custom-search").val(valueTemp);
        // $("#edit-aos").val(value);
      }

      $("#edit-name").keyup(textthaydoi);
    }
  };

})(jQuery, Drupal);
```
