# jQuery Form Validation

This jQuery extension provides an easy way to serialize HTML forms into JSON.

By default the serialization it's based on the submittable fields according to [W3C Successful controls](http://www.w3.org/TR/html401/interact/forms.html#h-17.13.2), but this is easily customizable to fit your needs.


## Installation

Include `jquery.form.serializer.js` after `jquery.js`.

<!--```html
<script src="jquery.js"></script>
<script src="jquery.form.serializer.js"></script>
```-->

## Usage

Based on a form like this one:

```html
<form id="my-form">
  <input type="hidden" name="token" value="ABC" />
  <input type="text" name="user[name]" value="John Doe" />
  <input type="text" name="user[email]" value="john@email.com" />
  <select name="user[country]">
    <option value="CL" selected>Chile</option>
  </select>
</form>
```

Serialize the form with the default options:

```javascript
$("#my-form").getSerializedForm();

// =>
{
  token: "ABC",
  user: {
    name: "John Doe",
    email: "john@email.com",
    country: "CL"
  }
}
```

## Customization

The submittable fields are selected according to these default options:

```javascript
$.fn.getSerializedForm.submittable = {
  selector: 'input, select, textarea',
  filters: {
    enabled: function() {
      return $(this).is(":enabled");
    },
    checked: function() {
      if ($(this).is(":checkbox, :radio")) {
        return $(this).is(":checked");
      } else {
        return true;
      }
    }
  }
};
```

The `selector` it's used to get a first set of fields to process.

The `filters` are a key-value set of functions that are passed to the initial matched set using the [jQuery's filter function](http://api.jquery.com/filter/). **NOTE:** Any filter with value `false`, `null` or `undefined` will be ignored.

You can replace these settings for a global customization, or you can pass the options on `getSerializedForm` to change the settings only for that call.

For example, to always include disabled fields:

```javascript
$.fn.getSerializedForm.submittable.filters.enabled = false;
```

To include disabled fields only for this call:

```javascript
$("#my-form").getSerializedForm({
  submittable: {
    filters: {
      enabled: false
    }
  }
});
```

Tests
-----

Open `./test/index.html` on any browser.
