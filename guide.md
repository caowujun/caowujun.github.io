# 西川睡眠项目引导

_made by 2022.7.29_

---

## 1. 路由

- 我们采用的是前端过滤的做法，后台返回一个role key（待定），前端可能有100个路由，然后这100个路由经过过滤，返回50个路由，只有这50个路由以内的，才可以跳转访问。
目前mock数据的testList是所有能访问的路由，注意下面这种父子菜单的，必须是父级路由也加。
```TypeScript
  '/contentManage', //コンテンツ一覧画面,  '/contentManage',
  '/contentManage/SCRN-C01-01-01/index',
```

- 在router中，NK-Admin-VUE\src\router\index.ts，定义了所有的路由。
这里分2种。（注意，路由的name不能相同）

    -  单路由。
    ①必须使用children，不然component没法设置layout，路由跳转后变全屏了。
    ②需要注意<code>showMainRoute: true</code>必须加，不然无法跳转。
    ③hidden的意思是不在左侧菜单显示。
    ④跳转的时候，path或者name采取第一层，比如<code>name: 'SCRN-C01-07-02'</code>,它会自动调用<code>/SCRN-C01-07-02/index</code>
    ```TypeScript
  {
    path: '/SCRN-C01-07-02',
    name: 'SCRN-C01-07-02',
    redirect: '/SCRN-C01-07-02/index',//作用是访问上面的path会直接跳转。
    component: Layout,
    meta: { hidden: true, showMainRoute: true },
    children: [
      {
        path: 'index',
        name: 'SCRN-C01-07-02-index',
        component: () => import('@/views/SCRN-C01-07-02/index.vue'),
        meta: {
          title: t('login.forgetPwdTitle')
        }
      }
    ]
  },
    ```

    - 父子路由。
    ①list和add 路由是同一层的。

    ```TypeScript
  {
    path: '/contentManage',
    component: Layout,
    // redirect: '/contentManage/applicationWaitingList',
    name: 'contentManage',
    meta: {
      title: t('menu.contentManage'),
      icon: 'ion:book'
    },
    children: [
      {
        path: 'SCRN-C01-01-01/index',
        name: 'SCRN-C01-01-01',
        component: () => import('@/views/SCRN-C01-01-01/index.vue'),
        meta: {
          title: t('menu.contentList')
        }
      },
      {
        path: 'SCRN-C01-01-02/index',
        name: 'SCRN-C01-01-02',
        component: () => import('@/views/SCRN-C01-01-02/index.vue'),
        meta: {
          title: t('menu.applicationWaitingList')
        }
      },
      {
        path: 'SCRN-C01-01-05/index',
        name: 'SCRN-C01-01-05',
        component: () => import('@/views/SCRN-C01-01-05/index.vue'),
        meta: {
          title: t('contentManage.addTitle'),
          hidden: true,
          showMainRoute: true
        }
      },
      {
        path: 'SCRN-C01-01-03/index',
        name: 'SCRN-C01-01-03',
        component: () => import('@/views/SCRN-C01-01-03/index.vue'),
        meta: {
          title: t('menu.approvalWaitingList')
        }
      },
      {
        path: 'SCRN-C01-01-04/index',
        name: 'SCRN-C01-01-04',
        component: () => import('@/views/SCRN-C01-01-04/index.vue'),
        meta: {
          title: t('menu.deliveryList')
        }
      }
    ]
  },
    ```

## 2. 多语言文件

- 如何在代码种显示多语言key对应的文本？在vscode的setting文件中，设定displayLanguage
    ```TypeScript
    "i18n-ally.displayLanguage": "ja",
    ```

- 如何增加多语言文件？"NK-Admin-VUE\src\locales\"文件夹下，有几个文件，文件名字对应上面的displayLanguage。
ja.ts是一个入口文件，内容如下，如果有新增需求模仿即可。

    ```TypeScript
    import error from './ja-JP/error'
    import menu from './ja-JP/menu'
    import jaJP from './ja-JP/ja-JP'

    export default {
    ...error,
    ...menu,
    ...jaJP
    }

    ```