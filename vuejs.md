## Overview
Backend used vuejs1 while frontend & cpanel used vuejs2 due to MaGIC header.

## Vuejs1 to Vuejs2
Here are few differences when using vuejs1 vs vuejs2

### passing data from view to vuejs
Vuejs1 method:
```html
<input type="hidden" v-model="portfolioId" value="<?php echo $model->id ?>" />
```

Vuejs2 method:
```html
<input type="hidden" name="portfolioId" :value="portfolioId = '<?php echo $model->id ?>'" />
```

### on ready:
Vuejs1 method:

```js
ready: function(){this.fetchData(0);},
```

Vuejs2 method:

```js
mounted: function(){this.fetchData(0);},
```