## Drawer

例えば、巨大なフォームを持っていたり、`terms & conditions` のようなものを表示するためのスペースが必要な場合、`Drawer` は `Dialog` とほぼ同じ API を持っていますが、ユーザーエクスペリエンスが異なります。

:::tip

#### Translation needed

Since v-model is natively supported for all components, `visible.sync` has been deprecated, use `v-model="visibilityBinding"` to control the visibility of the current drawer.
:::

### 基本的な使い方

一時的に Drawer を多方向から呼び出す

:::demo `Drawer` には `Dialog` のように `model-value` を設定して `Drawer` 自体の表示を制御する必要があります。`title` は名前付きスロットで、タイトルは `title` という属性を使って設定することもできます。デフォルトでは、`Drawer`はブラウザウィンドウの**30%**の大きさの**右隅から**左隅に向かって展開します。このデフォルトの動作を変更するには、`direction` と `size` 属性を設定します。このショーケースでは `before-close` API の使い方も紹介しました。さらに詳しく知りたい場合は Attributes セクションもご覧ください。

```html
<el-radio-group v-model="direction">
  <el-radio label="ltr">left to right</el-radio>
  <el-radio label="rtl">right to left</el-radio>
  <el-radio label="ttb">top to bottom</el-radio>
  <el-radio label="btt">bottom to top</el-radio>
</el-radio-group>

<el-button @click="drawer = true" type="primary" style="margin-left: 16px;">
  open
</el-button>

<el-drawer
  title="I am the title"
  v-model="drawer"
  :direction="direction"
  :before-close="handleClose"
>
  <span>Hi, there!</span>
</el-drawer>

<script>
  export default {
    data() {
      return {
        drawer: false,
        direction: 'rtl',
      }
    },
    methods: {
      handleClose(done) {
        this.$confirm('Are you sure you want to close this?')
          .then((_) => {
            done()
          })
          .catch((_) => {})
      },
    },
  }
</script>
<!--
<setup>

  import { defineComponent, ref } from 'vue';
  import { ElMessageBox } from 'element-plus';

  export default defineComponent({
    setup() {

      const drawer = ref(false);
      const direction = ref('rtl');
      const handleClose = (done) => {
        ElMessageBox
          .confirm('Are you sure you want to close this?')
          .then((_) => {
            done();
          })
          .catch((_) => {});
      };
      return {
        drawer,
        direction,
        handleClose,
      };
    },
  });

</setup>
-->
```

:::

### タイトルなし

タイトルが不要になったら、引き出しからタイトルを外すことができます。

:::demo withHeader`属性を**false**に設定すると、タイトルを削除することができます。アクセスしやすいようにしたい場合は、`title`属性を設定してください。

```html
<el-button @click="drawer = true" type="primary" style="margin-left: 16px;">
  open
</el-button>

<el-drawer title="I am the title" v-model="drawer" :with-header="false">
  <span>Hi there!</span>
</el-drawer>

<script>
  export default {
    data() {
      return {
        drawer: false,
      }
    },
  }
</script>
<!--
<setup>

  import { defineComponent, ref } from 'vue';

  export default defineComponent({
    setup() {
      return {
        drawer: ref(false),
      };
    },
  });

</setup>
-->
```

:::

### カスタマイズコンテンツ

ダイアログと同様に、`Drawer`はあなたが望むように多くの多様なインタラクションを行うことができる。

:::demo

```html
<el-button type="text" @click="table = true"
  >Open Drawer with nested table</el-button
>
<el-button type="text" @click="dialog = true"
  >Open Drawer with nested form</el-button
>
<el-drawer
  title="I have a nested table inside!"
  v-model="table"
  direction="rtl"
  size="50%"
>
  <el-table :data="gridData">
    <el-table-column property="date" label="Date" width="150"></el-table-column>
    <el-table-column property="name" label="Name" width="200"></el-table-column>
    <el-table-column property="address" label="Address"></el-table-column>
  </el-table>
</el-drawer>

<el-drawer
  title="I have a nested form inside!"
  :before-close="handleClose"
  v-model="dialog"
  direction="ltr"
  custom-class="demo-drawer"
  ref="drawer"
>
  <div class="demo-drawer__content">
    <el-form :model="form">
      <el-form-item label="Name" :label-width="formLabelWidth">
        <el-input v-model="form.name" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="Area" :label-width="formLabelWidth">
        <el-select
          v-model="form.region"
          placeholder="Please select activity area"
        >
          <el-option label="Area1" value="shanghai"></el-option>
          <el-option label="Area2" value="beijing"></el-option>
        </el-select>
      </el-form-item>
    </el-form>
    <div class="demo-drawer__footer">
      <el-button @click="cancelForm">Cancel</el-button>
      <el-button
        type="primary"
        @click="$refs.drawer.closeDrawer()"
        :loading="loading"
        >{{ loading ? 'Submitting ...' : 'Submit' }}</el-button
      >
    </div>
  </div>
</el-drawer>

<script>
  export default {
    data() {
      return {
        table: false,
        dialog: false,
        loading: false,
        gridData: [
          {
            date: '2016-05-02',
            name: 'Peter Parker',
            address: 'Queens, New York City',
          },
          {
            date: '2016-05-04',
            name: 'Peter Parker',
            address: 'Queens, New York City',
          },
          {
            date: '2016-05-01',
            name: 'Peter Parker',
            address: 'Queens, New York City',
          },
          {
            date: '2016-05-03',
            name: 'Peter Parker',
            address: 'Queens, New York City',
          },
        ],
        form: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: '',
        },
        formLabelWidth: '80px',
        timer: null,
      }
    },
    methods: {
      handleClose(done) {
        if (this.loading) {
          return
        }
        this.$confirm('Do you want to submit?')
          .then((_) => {
            this.loading = true
            this.timer = setTimeout(() => {
              done()
              // animation takes time
              setTimeout(() => {
                this.loading = false
              }, 400)
            }, 2000)
          })
          .catch((_) => {})
      },
      cancelForm() {
        this.loading = false
        this.dialog = false
        clearTimeout(this.timer)
      },
    },
  }
</script>
<!--
<setup>

import {defineComponent, reactive, toRefs } from 'vue';
import { ElMessageBox } from 'element-plus';

  export default defineComponent({
    setup() {

      const state = reactive({
        table: false,
        dialog: false,
        loading: false,
        gridData: [
          {
            date: '2016-05-02',
            name: 'Peter Parker',
            address: 'Queens, New York City',
          },
          {
            date: '2016-05-04',
            name: 'Peter Parker',
            address: 'Queens, New York City',
          },
          {
            date: '2016-05-01',
            name: 'Peter Parker',
            address: 'Queens, New York City',
          },
          {
            date: '2016-05-03',
            name: 'Peter Parker',
            address: 'Queens, New York City',
          },
        ],
        form: {
          name: '',
          region: '',
          date1: '',
          date2: '',
          delivery: false,
          type: [],
          resource: '',
          desc: '',
        },
        formLabelWidth: '80px',
        timer: null,
      });

      const handleClose = (done) => {
        if (state.loading) {
          return;
        }
        ElMessageBox
          .confirm('Do you want to submit?')
          .then((_) => {
            state.loading = true;
            state.timer = setTimeout(() => {
              done();
              // 动画关闭需要一定的时间
              setTimeout(() => {
                state.loading = false;
              }, 400);
            }, 2000);
          })
          .catch((_) => {});
      };

      const cancelForm = () => {
        state.loading = false;
        state.dialog = false;
        clearTimeout(state.timer);
      };

      return {
        ...toRefs(state),
        handleClose,
        cancelForm,
      };
    },
  });

</setup>
-->
```

:::

### 入れ子にした Drawer

`Dialog` のように `Drawer` を複数のレイヤーで構成することもできる。
:::demo 異なるレイヤーに複数の Drawer が必要な場合は、`append-to-body` 属性を **true** に設定しなければなりません。

```html
<el-button @click="drawer = true" type="primary" style="margin-left: 16px;">
  open
</el-button>

<el-drawer title="I'm outer Drawer" v-model="drawer" size="50%">
  <div>
    <el-button @click="innerDrawer = true">Click me!</el-button>
    <el-drawer
      title="I'm inner Drawer"
      :append-to-body="true"
      :before-close="handleClose"
      v-model="innerDrawer"
    >
      <p>_(:зゝ∠)_</p>
    </el-drawer>
  </div>
</el-drawer>

<script>
  export default {
    data() {
      return {
        drawer: false,
        innerDrawer: false,
      }
    },
    methods: {
      handleClose(done) {
        this.$confirm('You still have unsaved data, proceed?')
          .then((_) => {
            done()
          })
          .catch((_) => {})
      },
    },
  }
</script>
<!--
<setup>

  import { defineComponent, ref } from 'vue';
  import { ElMessageBox } from 'element-plus';

  export default defineComponent({
    setup() {

      const drawer = ref(false);
      const innerDrawer = ref(false);
      const handleClose = (done) => {
        ElMessageBox
          .confirm('You still have unsaved data, proceed?')
          .then((_) => {
            done();
          })
          .catch((_) => {});
      };
      return {
        drawer,
        innerDrawer,
        handleClose,
      };
    },
  });

</setup>
-->
```

:::

:::tip

Drawer 内のコンテンツは遅延レンダリングされるべきであり、これは Drawer 内のコンテンツが初期レンダリングのパフォーマンスに影響を与えないことを意味します。したがって、DOM の操作は `ref` を介して行うか、`open` イベントが発生した後に行うべきである。

:::

:::tip

Drawer は `destroyOnClose` という API を提供しています。これはフラグ変数であり、Drawer が閉じられた後に Drawer 内の子コンテンツを破棄することを示します。Drawer が開くたびに `mounted` ライフサイクルを呼び出す必要がある場合にこの API を利用することができます。

:::

### Drawer 属性

| Parameter             | Description                                                                                                                                                                                                                                           | Type                                                                                                                                                  | Acceptable Values     | Defaults |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | -------- |
| model-value / v-model | Drawer を表示する場合は、                                                                                                                                                                                                                             | boolean                                                                                                                                               | —                     | false    |
| append-to-body        | Drawer が DocumentBody 要素に挿入されている場合のコントロールは、ネストされた Drawer はこのパラメータを**true**に割り当てなければなりません。                                                                                                         | boolean                                                                                                                                               | —                     | false    |
| lock-scroll           | Drawer 表示中にボディのスクロールを無効にするかどうか                                                                                                                                                                                                 | boolean                                                                                                                                               | —                     | true     |
| before-close          | 設定されている場合は、終了処理を停止します。                                                                                                                                                                                                          | function(done), done is function type that accepts a boolean as parameter, calling done with true or without parameter will abort the close procedure | —                     | —        |
| close-on-click-modal  | Drawer を閉じることができるかどうか                                                                                                                                                                                                                   | boolean                                                                                                                                               | —                     | true     |
| close-on-press-escape | ESC を押して Drawer を閉じることができるかどうかを示す。                                                                                                                                                                                              | boolean                                                                                                                                               | —                     | true     |
| open-delay            | Time(milliseconds) before open                                                                                                                                                                                                                        | number                                                                                                                                                | —                     | 0        |
| close-delay           | Time(milliseconds) before close                                                                                                                                                                                                                       | number                                                                                                                                                | —                     | 0        |
| custom-class          | Drawer の追加クラス名                                                                                                                                                                                                                                 | string                                                                                                                                                | —                     | —        |
| destroy-on-close      | Drawer が閉じた後に Children が破壊されるべきかどうかを示す                                                                                                                                                                                           | boolean                                                                                                                                               | -                     | false    |
| modal                 | シャドウイングレイヤーを表示するか                                                                                                                                                                                                                    | boolean                                                                                                                                               | —                     | true     |
| direction             | Drawer の開き方向                                                                                                                                                                                                                                     | Direction                                                                                                                                             | rtl / ltr / ttb / btt | rtl      |
| show-close            | Drawer の右上に閉じるボタンを表示するようにした                                                                                                                                                                                                       | boolean                                                                                                                                               | —                     | true     |
| size                  | Drawer のサイズ, ドローワが水平モードの場合は幅プロパティ, そうでない場合は高さプロパティ, サイズが `number` 型の場合はピクセル単位でサイズを記述します; サイズが `string` 型の場合は `x%` 記法を用います, それ以外の場合はピクセル単位で解釈されます | number / string                                                                                                                                       | -                     | '30%'    |
| title                 | Drawer のタイトルは、スロットの名前を指定して設定することもできます。                                                                                                                                                                                 | string                                                                                                                                                | —                     | —        |
| withHeader            | デフォルトは true で、withHeader が false に設定されている場合は `title attribute` と `title slot` の両方が動作しません。                                                                                                                             | boolean                                                                                                                                               | -                     | true     |
| modal-class           | シャドウレイヤーの追加クラス名                                                                                                                                                                                                                        | string                                                                                                                                                | —                     | —        |
| z-index               | set z-index                                                                                                                                                                                                                                           | number                                                                                                                                                | -                     | -        |

### Drawer スロット

| Name  | Description               |
| ----- | ------------------------- |
| —     | Drawer の内容             |
| title | Drawer タイトルセクション |

### Drawer メソッド

| Name        | Description                                                     |
| ----------- | --------------------------------------------------------------- |
| handleClose | Drawer を閉じるには、このメソッドは `before-close` を呼び出す。 |

### Drawer イベント

| Event Name | Description                                                         | Parameter |
| ---------- | ------------------------------------------------------------------- | --------- |
| open       | Drawer のオープニングアニメーションが始まる前にトリガーされます。   | —         |
| opened     | Drawer のオープニングアニメーションが終わった後にトリガーされます。 | —         |
| close      | Drawer のクロージングアニメーションが始まる前にトリガーされます。   | —         |
| closed     | Drawer のクロージングアニメーションが終わった後にトリガーされます。 | —         |
