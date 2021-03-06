## 如何描述与输出动态表单的一种思路.

总体来说用 jsonSchema 来描述动态表单中 item 的类型.
譬如:

```type === 'boolean'```, 表示 item 是一个单选框, 用 switch 组件渲染

```type === 'number'```, 表示 item 是一个数字选框,

```final```字段描述只读的选框,

```enum```字段描述下拉框,

```type === 'array'```, 表示它是一个下拉多选框.

```description```字段用来描述该输入框的提示性文字.

## 缺点.
1. 需要后端配合在接口里输出详细的 jsonSchema.
2. 描述能力有限,对于复杂型表单,需要继续扩展.
3. 无法描述表单 item 的校验规则.
