# vue-right-tree
>  A right streight  diagram tree component base on vue

### Demo
![](https://github.com/tangtongda/vue-right-tree/blob/master/static/demo.png?raw=true)

### 1. Install unique depency : LeaderLine
**npm:**
[https://www.npmjs.com/package/leader-line-vue](https://www.npmjs.com/package/leader-line-vue)
**doc:**
[https://anseki.github.io/leader-line/](https://anseki.github.io/leader-line/)

```shell
npm i leader-line-vue
```
### 2. Copy the "RightTree.vue" to your vue projects

> For more custom commercial works,i recommend you to use this way to use the tree components.

### 3. Import the components in anywhere

```html
<template>
  <div>
    <right-tree
      v-if="list && list.length"
      :list="list"
      :showfields="showFields"
    ></right-tree>
  </div>
</template>
<script>
import RightTree from '../components/RightTree'
export default {
    components:{
        RightTree,
    },
    data(){
        return{
            list: [//datasource
                {
                    id: '1',
                    name: 'span1',
                    serviceId: 'service1',
                    children: [
                        {
                            id: '1-1',
                            name: 'user',
                            serviceId: 'service-user',
                            children: [
                                {
                                    id: '1-1-1',
                                    name: 'shop',
                                    serviceId: '18',
                                    children: [
                                        {
                                            id: '1-1-1-1',
                                            name: 'common',
                                            serviceId: 'common-service',
                                        },
                                    ],
                                },
                                {
                                    id: '1-1-2',
                                    name: 'account1',
                                    serviceId: 'account-service',
                                },
                                {
                                    id: '1-1-3',
                                    name: 'account2',
                                    serviceId: 'account-service',

                                },
                                {
                                    id: '1-1-4',
                                    name: 'account3',
                                    serviceId: 'account-service',
                                },
                            ],
                        },
                        {
                            id: '1-2',
                            name: 'truck',
                            serviceId: 'truck-pay',
                            work: 'web',
                        },
                    ],
                },
            ],
            showFields: [
                {
                    name: '服务名称：',
                    key: 'name',
                },
                {
                    name: '服务编号：',
                    key: 'serviceId',
                },
            ],
        }
    },
    methods:{

    },
    
}
</script>
```
### 4. Q&A
#### 1.Why the LeaderLine cannot roll with the page?
A: Because the svg element is mounted on the html body. So, when you scroll the page by dom in body, svg element still fixed.You can just cancel the scroll but let the body scroll to control.