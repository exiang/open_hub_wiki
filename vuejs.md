## Overview
Backend used Vuejs1 while frontend & cpanel used Vuejs2 due to MaGIC header.

## Vuejs1 vs Vuejs2
Here are few differences when using Vuejs1 vs Vuejs2

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