# v-drop

## Install
``` bash
npm install v-drop --save-dev
```

``` bash
import vdrop from 'v-drop';
Vue.component('my-component', {
    components: {
        vdrop
    }
});
```

## Available props

| Event         |Type           | Default    | Description                                         |
|---------------|---------------|------------|-----------------------------------------------------|
| min           |Number         |   1000     | 最小值                                               |
| max           |Number         |   10000    | 最大值                                               |
| speed         |Number         |   100      | 滑动距离                                             |
| model         |Number         |            | 返回值                                               |

``` html
<vdrop :min="min" :max="max" :speed="speed" v-model="money"></vdrop>
```
