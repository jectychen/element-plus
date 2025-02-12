## ColorPicker

ColorPicker es un selector de color que soporta varios formatos de color.

### Uso básico

:::demo ColorPicker requiere una variable de tipo `string` para ser enlazada a `v-model`.

```html
<div class="block">
  <span class="demonstration">Especifica valor por defecto</span>
  <el-color-picker v-model="color1"></el-color-picker>
</div>
<div class="block">
  <span class="demonstration">No especifica valor por defecto</span>
  <el-color-picker v-model="color2"></el-color-picker>
</div>

<script>
  export default {
    data() {
      return {
        color1: '#409EFF',
        color2: null,
      }
    },
  }
</script>
<!--
<setup>

  import { defineComponent, ref } from 'vue';

  export default defineComponent({
    setup() {
      const color1 = ref('#409EFF');
      const color2 = ref(null);
      return {
        color1,
        color2,
      };
    },
  });

</setup>
-->
```

:::

### Alpha

:::demo ColorPicker soporta selección de canales alpha. Para activarlo sólo agregue el atributo `show-alpha`.

```html
<el-color-picker v-model="color" show-alpha></el-color-picker>

<script>
  export default {
    data() {
      return {
        color: 'rgba(19, 206, 102, 0.8)',
      }
    },
  }
</script>
<!--
<setup>

  import { defineComponent, ref } from 'vue';

  export default defineComponent({
    setup() {
      const color = ref('rgba(19, 206, 102, 0.8)');
      return {
        color,
      };
    },
  });

</setup>
-->
```

:::

### Colores predefinidos

:::demo ColorPicker soporta opciones de color predefinidas

```html
<el-color-picker v-model="color" show-alpha :predefine="predefineColors">
</el-color-picker>

<script>
  export default {
    data() {
      return {
        color: 'rgba(255, 69, 0, 0.68)',
        predefineColors: [
          '#ff4500',
          '#ff8c00',
          '#ffd700',
          '#90ee90',
          '#00ced1',
          '#1e90ff',
          '#c71585',
          'rgba(255, 69, 0, 0.68)',
          'rgb(255, 120, 0)',
          'hsv(51, 100, 98)',
          'hsva(120, 40, 94, 0.5)',
          'hsl(181, 100%, 37%)',
          'hsla(209, 100%, 56%, 0.73)',
          '#c7158577',
        ],
      }
    },
  }
</script>
<!--
<setup>

  import { defineComponent, ref } from 'vue';

  export default defineComponent({
    setup() {
      const color = ref('rgba(255, 69, 0, 0.68)');
      const predefineColors = ref([
        '#ff4500',
        '#ff8c00',
        '#ffd700',
        '#90ee90',
        '#00ced1',
        '#1e90ff',
        '#c71585',
        'rgba(255, 69, 0, 0.68)',
        'rgb(255, 120, 0)',
        'hsv(51, 100, 98)',
        'hsva(120, 40, 94, 0.5)',
        'hsl(181, 100%, 37%)',
        'hsla(209, 100%, 56%, 0.73)',
        '#c7158577',
      ]);
      return {
        color,
        predefineColors,
      };
    },
  });

</setup>
-->
```

:::

### Sizes

:::demo

```html
<el-color-picker v-model="color"></el-color-picker>
<el-color-picker v-model="color" size="medium"></el-color-picker>
<el-color-picker v-model="color" size="small"></el-color-picker>
<el-color-picker v-model="color" size="mini"></el-color-picker>

<script>
  export default {
    data() {
      return {
        color: '#409EFF',
      }
    },
  }
</script>
<!--
<setup>

  import { defineComponent, ref } from 'vue';

  export default defineComponent({
    setup() {
      const color = ref('409EFF');

      return {
        color,
      };
    },
  });

</setup>
-->
```

:::

### Atributos

| Atributo              | Descripción                                                        | Tipo    | Valores aceptados     | Por defecto                                               |
| --------------------- | ------------------------------------------------------------------ | ------- | --------------------- | --------------------------------------------------------- |
| model-value / v-model | valor enlazado                                                     | string  | —                     | —                                                         |
| disabled              | especifica si se deshabilita el ColorPicker                        | boolean | —                     | false                                                     |
| size                  | tamaño del ColorPicker                                             | string  | —                     | medium / small / mini                                     |
| show-alpha            | especifica si se muestra el control deslizante para el valor alpha | boolean | —                     | false                                                     |
| color-format          | formato de color del `v-model`                                     | string  | hsl / hsv / hex / rgb | hex (si show-alpha es false)/ rgb (si show-alpha es true) |
| popper-class          | nombre de clase para el dropdown del ColorPicker                   | string  | —                     | —                                                         |
| predefine             | opciones de colores predefinidas                                   | array   | —                     | —                                                         |

### Eventos

| Nombre de Evento | Descripción                                     | Parametros             |
| ---------------- | ----------------------------------------------- | ---------------------- |
| change           | se dispara cuando el valor del input cambia     | valor del color        |
| active-change    | se dispara cuando el actual color activo cambia | valor del color activo |
