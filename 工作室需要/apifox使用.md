# apifox使用

- 接口用例管理

  - 一个接口一般有多个用例，正确的，格式错误的，参数错误的等等，这些用例我们都可以使用apifox的用例管理来帮我们**快速的校验一个接口是否合格**，是否满足所有的预期
  -  在`自动化测试`——`测试用例`处导入接口，并测试，可以生成测试报告

- **接口文档**

  - 概念

    - **接口设计**：即 **新建接口** 界面或接口详情里的 **编辑** 界面，用途是 **定义接口文档规范**，而不是 **运行** 接口，所以该界面是只能定义接口基本信息、`参数名`及参数说明等，而不能设置`参数值`。**参数值**、**前置脚本/后置脚本** 等信息请在`接口运行`界面或`接口用例`界面填写。
    - **接口运行**：即接口详情里的 **运行** 界面，用途是 **临时调试接口**，**运行** 完后，需要点击`保存为用例`，才能将填写的 **参数值**、**前置脚本/后置脚本** 等信息保存下来；否则关闭 tab 后，这些信息将会丢失。

  - 用法

    - 点击左侧搜索框旁边的 `+` 号按钮即可打开新建窗口，也可使用 快捷键Ctrl+ N。
    - 在打开的窗口中，直接定义接口相关信息。

  - 参数解释

    - 接口路径

      - 以斜杠`/`起始的接口 path 部分，如`/pets`、`/pets/{id}`。
      - 注意
        1. **接口路径** 建议`不要包含 HTTP 协议及域名`，这部分建议在 环境管理 的`前置URL`里设置，接口调试时的 URL 会自动加上当前环境的`前置URL`。
        2. 特殊情况需在接口路径要带上`HTTP 协议及域名`的，系统也能支持，但不建议这么做。接口调试时，系统如检测到接口路径是以`http://`或`https://`起始的，会自动忽略当前环境里前置 URL。
        3. Apifox 中的 `Path 参数`是以大括号包裹起来表示，而非冒号起始表示。**正确示例**：`/pets/{id}`，**错误示例**`/pets/:id`。
        4. **接口路径** 不可包含`Query 参数`（即 URL 中 `?`后的参数），Query 参数在下方`请求参数`部分填写。

    - 请求参数

      - Params参数

        - 包含 `Query 参数`和 `Path 参数`两部分。
          - **Query 参数**：即 URL 中 `?`后的参数。
          - **Path 参数**：自动提取`接口路径`中大括号包裹起来的参数，如`/pets/{id}`中的的`{id}`即表示名为`id`的 Path 参数。

      - Body参数

        - #### Body 参数类型

          - **none**：无 body 参数。
          - **form-data**：即 Content-Type 为`multipart/form-data`。
          - **x-www-form-urlencoded**：即 Content-Type 为`application/x-www-form-urlencoded`。
          - **json**：即 Content-Type 为 `application/json`。
          - **xml**：即 Content-Type 为 `application/xml`。
          - **binary**：发送文件类数据时使用。
          - **raw**：发送其他文本类数据时使用。

        - 注意

          - Body 参数类型为`json`或`xml`时，需要设置数据结构，并且数据结构可以引用`数据模型`
          - 接口发送请求的时候会根据`Body 参数类型`自动在请求`Header`加上对应的`Content-Type`，无需手动设置。
          - 若需要手动设置`Header`中的`Content-Type`，则其值必须和`Body 参数类型`相匹配，否则系统会自动忽略掉手动设置的`Content-Type`。
            1. 示例：如 Body 参数类型为`form-data`，手动设置`Content-Type`的值为`multipart/form-data; charset=GBK`是有效的；但如果把值设置为`application/json`则会被系统忽略掉，因为和参数类型不匹配。
            2. Body 参数类型为`raw`时，手动设置`Content-Type`的值不受限制。

    - 返回响应

      - 返回响应定义主要包含以下几部分
        - 接口返回的 HTTP 状态码
        - 返回内容的数据格式：`JSON`、`XML`、`HTML`、`Raw`、`Binary`
        - 数据结构：仅`JSON`、`XML`可配置数据结构
      - 注意
        - 当一个接口在不同情况下会返回不同数据结构时，可设置多个`返回响应`。点击`返回响应`模块右上方的`+ 新建`即可添加。
        - 定义好数据结构后，`接口调试`时，系统会自动校验返回的数据是否符合定义的数据结构，非常方便
        - 定义好数据结构后，`使用 mock 功能`时，系统会自动根据定义的数据结构 mock 出非常人性化的数据，非常方便

    - 公共响应

      - `公共响应`主要用来实现返回响应的复用。

        通常不同接口在某些情况下会返回相同的数据结构，如`资源不存在(404)`、`没有访问权限(401)`等，这些建议设置为`公共响应`，避免重复编写，方便统一管理。

        设置方法：打开`项目设置`->`公共响应`，在这里管理公共响应。

        

- 接口调试

  - 使用方法：打开接口文档，点击`运行` tab即可。

  - 保存为用例

    - `保存为用例` 是将当前填写的参数保存起来，方便下次或者其他人用来调试接口。保存为用例后，`接口用例` 会显示在左侧树状菜单里接口的下一级（如上图）。
    - 注意
      - 接口用例是非常有用的。从团队协作的场景出发，建议每次`运行`后都`保存为用例`，后续用`接口用例`来调试接口是非常高效的。
      - 通常一个接口会有多种情况用例，比如`参数正确`用例、`参数错误`用例、`数据为空`用例、`不同数据状态`用例等等。

  - 接口参数

    - 接口路径、参数名会自动从 `修改文档` 读取，无需手动输入
    - 参数值默认读取 `修改文档` 里的 `示例值`，也可手动修改，进行调试
    - 填写好参数后，点击`发送`按钮即可运行。

  - 前置操作/后置操作

    - 项目维度：可以在 `项目概览` 中设置，会对整个项目下的接口/接口用例生效。

    - 分组维度：点击对应的 `分组` 即可设置，会对分组下的接口/接口用例生效。

    - 单个接口：在 `接口文档-运行` 页设置 `前置操作/后置操作` ，需要 `保存为接口用例` ，点击 `保存` 不会被保存在接口文档中，也不会对该接口下面的 `接口用例` 生效。

    - 单个接口用例：在 `接口用例` 中设置 `前置操作/后置操作` ，只对本 `接口用例` 生效

    - #### 断言

      `后置操作`支持添加`断言`，可对接口返回的数据（或响应时间）设置断言，判断是否符合预期

    - #### 提取变量

      `后置操作`支持添加`提取变量`，可从接口返回结果里提取数据，设置到变量（临时变量/环境变量/全局变量），方便其他接口运行的时候直接使用。

    - ####  数据库操作

      - `前置操作`、`后置操作`支持添加`数据库操作`，可读写数据库数据，查询结果可在接口请求参数、断言、自定义脚本等场景中使用。目前支持`MySQL`、`SQL Server`、`Oracle`、`PostgreSQL`，未来会支持更多数据库类型。

        

  - 校验响应

    - `校验响应` 是一个高效的测试工具，以 `接口文档-修改文档` 页面内填写的 `返回响应` 作为判断标准，与 `请求接口` 的获得的返回值进行对比。

    - `校验响应` 的校验范围：

      - 接口返回的 HTTP 状态码
      - 返回内容的数据格式：`JSON`、`XML`、`HTML`、`Raw`、`Binary`
      - 数据结构：仅`JSON`、`XML`可配置数据结构

    - 如果上述 2 者一致，则显示 ”返回数据结构校验通过!“。说明真实的接口返回值是符合接口文档定义的，不需要人工核对，提高效率和准确性。

    - 当不一致时，就会报错并提示具体是哪部分不一致。此时可以选择修改 `接口文档-修改文档` 内的 `返回响应` ；也可以通知后端同学修改后端代码。

    - `校验响应` 开关默认打开。可以在界面左下角 `设置-通用-校验响应` 关闭全局开关，注意：全局开关只会对 `接口文档-运行` 生效，不会对已保存的 `接口用例` 生效。

      

- “数据模型”定义、引用

  - 数据结构

    - `数据结构` 和编程语言里的数据结构类似，主要使用在 接口设计 的`返回响应`和 json / xml 类型的`Body 参数`。

    - 编辑数据结构

      - 可以选择该字段是否必填

        ![img](https://cdn.apifox.cn/mirror-www/help/assets/img/api-schema-4.0268d239.png)

      - 可以选择该字段的数据类型![img](https://cdn.apifox.cn/mirror-www/help/assets/img/api-schema-5.43d854d9.png)

      - 可以编辑该字段的`Mock 设置`

      - 可以新增字段，或删除该字段![img](https://cdn.apifox.cn/mirror-www/help/assets/img/api-schema-6.0cbe1d22.png)

      - 可拖拽移动，改变字段之间的排序![img](https://cdn.apifox.cn/mirror-www/help/assets/img/api-schema-7.7332978f.png)

      

  - 在使用`数据模型`前，需要先建立`可复用的数据结构`。如下图，根据项目需要，可以先在`数据模型`下新建，也可以简单的管理不同数据模型间的关系。![img](https://cdn.apifox.cn/mirror-www/help/assets/img/api-schema-8.e8b6e369.png)

  - **注意：数据模型之间也可以相互引用**

  - 数据模型的引用

    - 在 接口设计 的`返回响应`和 json / xml 类型的`Body 参数`处，在`数据类型`处可以引用已经建立好的`数据模型`，如下图。![img](https://cdn.apifox.cn/mirror-www/help/assets/img/api-schema-9.c83e4778.png)

    - 当前引用的数据模型不符合要求，需要修改时，可以直接跳转到`数据模型`进行修改

    - 当下接口需要部分引用`数据模型`时，可以在引用的情况下修改，并且不影响原`数据模型`

      - 当不需要`数据模型`中的某个字段时，可以点击`隐藏字段`，则接口文档中就不会显示了
      - 当需要对`数据模型`中的某个字段，特殊编辑时，可以点击`取消关联`。当然后续也可以`恢复关联`
      - 可以引用多个`数据模型`，并支持`数据模型`之间拖拽排序

      

- ### Mock功能

  - 前端开发往往依赖于后端数据接口，在后端接口就绪之前，前端通常很难开工。Mock 功能就是用来解决这个问题的。有了 Mock 工具之后，前后端可以同步进入开发，后端接口出来之前，前端可以通过 Mock 功能来制造假数据接口来进行开发和调试。

  - 功能说明

    - Mock 功能可以根据`接口/数据结构定义`、`Mock规则配置`、`Mock 期望配置`，自动生成模拟数据，且使用者可以根据需要灵活构造各种结构的接口数据。
    - 通常情况 Apifox `零配置`即可生成非常人性化的 mock 数据：
      1. Apifox 根据接口定义里的数据结构、数据类型，自动生成 mock 规则。
      2. Apifox 内置 智能 Mock功能，根据字段名、字段数据类型，智能优化自动生成的 mock 规则。如：名称包含字符串`image`的`string`类型字段，自动 mock 出一个图片地址 URL；包含字符串`time`的`string`类型字段，自动 mock 出一个时间字符串；包含字符串`city`的`string`类型字段，自动 mock 出一个城市名。
      3. Apifox 根据内置规则（可关闭），可自动识别出图片、头像、用户名、手机号、网址、日期、时间、时间戳、邮箱、省份、城市、地址、IP 等字段，从而 Mock 出非常人性化的数据。
      4. 除了内置 mock 规则，用户还可以自定义规则库，满足各种个性化需求。支持使用 `正则表达式`、`通配符` 来匹配字段名自定义 mock 规则。

  - Mock请求URL

    - 请求`method`与接口定义的`method`保持一致。

    - 如你项目 ID 为`18600`，需要 mock 的接口 ID 为`89343`，路径为`/users/123`，请求`method`为`POST`，则实际 Mock URL 为：

      ```js
      // 本地 Mock 地址
      POST http://127.0.0.1:4523/m1/18600-0-default/users/123
      或
      POST http://127.0.0.1:4523/m2/18600-0-default/89343
      
      
      // 云端 Mock 地址
      POST https://mock.apifox.cn/m1/18600-0-default/users/123
      或
      POST https://mock.apifox.cn/m2/18600-0-default/89343
      ```

    - 默认情况下，定义好`接口/数据结构`后，无需做任何额外的配置，就可以通过上面的 URL 访问到自动 Mock 出来的数据接口了。

    - ##### Mock URL 说明

      - **本地 Mock**：

        - **路径模式** `http://127.0.0.1:4523/m1/{项目ID}-{版本编号}-{服务编号}/{接口路径}`
        - 示例：http://127.0.0.1:4523/m1/18600-0-0/users/123
        - **ID 模式** `http://127.0.0.1:4523/m2/{项目ID}-{版本编号}-{服务编号}/{接口ID}`
        - 示例：http://127.0.0.1:4523/m2/18600-0-0/84924

        **云端 Mock**

        - **路径模式** `https://mock.apifox.cn/m1/{项目ID}-{版本编号}-{服务编号}/{接口路径}`
        - 示例：https://mock.apifox.cn/m1/18600-0-0/users/123
        - **ID 模式** `https://mock.apifox.cn/m2/{项目ID}-{版本编号}-{服务编号}/{接口ID}`
        - 示例：https://mock.apifox.cn/m2/18600-0-0/84924

        **其他说明**

        - **项目 ID**：打开Apifox，进入“项目设置”查看
        - **版本编号**：默认版本编号为“0”，表示主版本。
        - **服务编号**：仅在项目使用了`多个服务`的时候才特殊指定，“default”表示默认服务（推荐，默认服务下不存在该接口时自动查询其他服务下同路径接口）。

      - 注意

        1. Mock 服务是在本地启动的，所以 URL 里的 ip 地址为`127.0.0.1`，如有其他设备需要访问 mock 数据，只需将`127.0.0.1`改成本机的内网 ip 即可。如果还是访问不了，请检查是否防火墙等限制了 mock 所用的`4523`端口。

        2. 如一个项目内，有多个接口拥有相同的

           ```
           method + path路径
           ```

           ，可使用如下 2 种方式指定接口，否则在会产生路径冲突。

           1. **接口路径模式**：需额外添加 Query 参数 `?apifoxApiId={接口ID}`.
           2. **接口 ID 模式**：无需任何处理

        3. 如接口路径不是以`/`起始的，则只能使用 **接口 ID 模式**，不能使用 **接口路径模式**。

        4. 打开 Apifox 就会默认启动 mock 服务，无需额外操作。

        5. Mock 服务的 `前置 URL` 是固定的，不能修改。

        

  - 自定义Mock规则

    - 数据结构定义 mock 规则
      - 定义数据结构的时候，可手动设置 mock 规则
    - 数据字段高级设置
      - 数据字段`高级设置`里设置的`最大值`、`最小值`、`枚举值`、`Partten`、`format`，也会作为 Mock 规则使用

  - #### 高级Mock

    - ##### **设置位置：`接口详情`-`高级 Mock`**

    - Mock 优先级说明

      请求 Mock 数据时，规则匹配优先级：高级 Mock 里的期望 > 自定义 Mock 脚本。

      如果匹配到了`高级 Mock 里的期望`，则不调用`自定义 Mock 脚本`。

    - Mock期望

      - ![截图](https://cdn.apifox.cn/mirror-www/help/assets/img/mock-custom-scripts-1.d8bb4518.png)

        

      - 配置项说明：

        - ![截图](https://cdn.apifox.cn/mirror-www/help/assets/img/mock-custom-scripts-2.6da1b11d.png)

        - `期望条件`：根据不同的请求参数，返回不同数据。如创建 2 个 `期望`：

          1. 请求参数`id`为`1`时，返回销售状态为`available`的数据。
          2. 请求参数`id`为`2`时，返回销售状态为`sold`的数据。

        - `期望条件`支持设置多个参数，多个参数`同时匹配`时才会匹配到该期望。

        - `期望条件`支持设置参数名和参数值之间的`比较`关系，包含：等于、小于、大于、存在、包含等

        - 若`期望条件`里的参数位置选择为`body`，则实际请求的 `body 请求类型`需要和该接口定义保持一致，如接口定义的 body 请求类型为`form-data`，则 mock 时该参数也需要放在`form-data`里。

        - `期望条件` ： json 类型的 body 支持使用`JSON Path` 匹配

          - 参数名以 $ 字符起始的，使用 JSON Path 来匹配
          - 参数名不是以 $ 字符起始的，直接匹配 JSON 第一级的属性名

        - `返回数据`：即接口请求返回的数据，支持 `mock.js` 、`Nunjucks` 语法，即可按一定的规则返回`动态数据`。

        - 支持自定义`返回 Header`、`返回 HTTP 状态码`、`返回延迟`。

          

    - Mock自定义脚本

      - 自定义脚本方式可获取用户请求的参数，可修改返回内容。

        注意：此处脚本仅用于 `高级mock` 的 `Mock 自定义脚本`，不能用于前后置脚本中。

      - 使用方法

        1. 首先开启此功能
        2. 使用 JavaScript 脚本修改返回的 JSON 数据，如图![截图](https://cdn.apifox.cn/mirror-www/help/assets/img/mock-custom-scripts-4.574ec33b.png)

        

  - #### 智能Mock

    - 当接口设计的返回 Response (或数据模型) 里的字段未配置 mock 规则时，系统会自动使用智能 Mock 规则来生成数据，以实现使用时`零配置`即可 mock 出非常人性化的数据。

    - #####  智能 Mock 规则配置

      设置位置：`项目设置`-`智能 Mock 设置`的`自定义规则`及`内置规则`。

      1. 自定义规则：用户可新建自定义规则，满足各种个性化需求。支持使用 `正则表达式`、`通配符` 来匹配字段名自定义 mock 规则。
      2. 内置规则：系统内置常用 mock 规则库，可自由决定是否开启内置规则。
      3. 优先级：`自定义规则`优先级高于`内置规则`，可添加自定义规则来覆盖系统内置规则。

    - ![Apifox Mock 设置自定义规则及内置规则](https://cdn.apifox.cn/mirror-www/help/assets/img/intelligent-mock-1.2c903beb.png)

      

  - Mock规则优先级

    - 数据字段在自动 Mock 数据时，实际执行的 Mock 规则优先级顺序如下：
      1. 接口详情`高级 Mock` 里设置的`期望`（根据接口参数匹配）。
      2. 数据结构的字段里设置的`Mock`规则。
      3. 数据结构的字段`高级设置`里设置的`最大值`、`最小值`、`枚举值`、`Partten`。
      4. `项目设置`-`智能 Mock 设置`的`自定义规则`。
      5. `项目设置`-`智能 Mock 设置`的`内置规则`。
      6. 数据结构里字段的`数据类型`。

  - ### Mock语法

    -  基本写法

      - | 写法              | 说明                                                         |
        | ----------------- | ------------------------------------------------------------ |
        | 以`@`起始的字符串 | 调用 Mock 语法规则生成对应的数据。 如生成的数据类型和定义的数据类型不一致，则会自动转换。 |
        | 非`@`起始的字符串 | 数据类型为`string`时，原样输出。 其他数据类型，会将字符串自动转换到对应的数据类型。 |
        | 特殊字符：null    | 数据类型允许为`null` 时，输出`null`。 否则自动转换，如数据类型为`string`，输出`"null"`。 |
        | 特殊字符：true    | 数据类型为`boolean` 时，输出`true`。 否则自动转换，如数据类型为`string`，输出`"true"。` |
        | 特殊字符：false   | 数据类型为`boolean` 时，输出`false`。 否则自动转换，如数据类型为`string`，输出`"false"。` |

        **自动转换** 是使用 javascript 语言默认数据转换方法进行转换。

        

    - Mock 语法

      - 基础类型

        | 分类   | 规则                           | 示例                    | 示例结果                                                |
        | ------ | ------------------------------ | ----------------------- | ------------------------------------------------------- |
        | 布尔值 | @boolean                       | @boolean                | false true true                                         |
        |        | @boolean( min, max, current )  | @boolean(1, 9, true)    | false false                                             |
        | 自然数 | @natural                       | @natural                | 5748399088025322 3295768519606992 6595528165924058      |
        |        | @natural( min )                | @natural(10000)         | 5492366038361662 2061541478134547                       |
        |        | @natural( min, max )           | @natural(60, 100)       | 99 66                                                   |
        | 整数   | @integer                       | @integer                | -7585865973372456 3913822053899244 5164580957595932     |
        |        | @integer( min )                | @integer(10000)         | 7304069372231661 6538343333105173                       |
        |        | @integer( min, max )           | @integer(60, 100)       | 99 -66                                                  |
        | 浮点数 | @float                         | @float                  | 4791951330736181 3310225074798816.5 -2562681950443352.5 |
        |        | @float( min )                  | @float(0)               | 460523639430244.3 5146972509822378                      |
        |        | @float( min, max )             | @float(60, 100)         | 91.85250094787806 66.1232254588188                      |
        |        | @float( min, max, dmin )       | @float(60, 100, 3)      | 66.4375611 78.50157676437885                            |
        |        | @float( min, max, dmin, dmax ) | @float(60, 100, 3, 5)   | 91.5811 91.27216                                        |
        | 单字符 | @character                     |                         | "z" "%" "V"                                             |
        |        | @character( pool )             | @character('lower')     | "x"                                                     |
        |        |                                | @character('upper')     | "R"                                                     |
        |        |                                | @character('number')    | "6"                                                     |
        |        |                                | @character('symbol')    | "#"                                                     |
        |        | @character( pool )             | @character('aeiou')     | "i" "o"                                                 |
        | 字符串 | @string                        | @string                 | "2u&x" "rUlmf" "5qfJp1"                                 |
        |        | @string( length )              | @string(5)              | "88yC6" "3^8VT"                                         |
        |        | @string( pool, length )        | @string('lower', 5)     | "xlmes"                                                 |
        |        |                                | @string('upper', 5)     | "SETVW"                                                 |
        |        |                                | @string('number', 5)    | "12751"                                                 |
        |        |                                | @string('symbol', 5)    | "%&(#*"                                                 |
        |        |                                | @string('aeiou', 5)     | "eeaeu"                                                 |
        |        | @string( min, max )            | @string(7, 10)          | "ZIuenM8" "DVu%]kEqC"                                   |
        |        | @string( pool, min, max )      | @string('lower', 1, 3)  | "m"                                                     |
        |        |                                | @string('upper', 1, 3)  | "XXA"                                                   |
        |        |                                | @string('number', 1, 3) | "8"                                                     |
        |        |                                | @string('symbol', 1, 3) | "["                                                     |
        |        |                                | @string('aeiou', 1, 3)  | "a"                                                     |

        

      - 正则表达式

        | 规则              | 示例                                           | 示例结果           |
        | ----------------- | ---------------------------------------------- | ------------------ |
        | @regexp( regexp ) | @regexp(/\d+/)                                 | "36436"            |
        |                   | @regexp(/\d{3,5}/)                             | "343"              |
        |                   | @regexp(/^[a-zA-Z][A-Za-z0-9_-.]+@gmail.com$/) | "ifa3dt@gmail.com" |

        - 注意
          - Apifox 版本号大于等于 `1.0.12` 才支持正则表达式。
          - regexp 参数必须以 `/` 起始和结尾。
          - 正则表达式参考文档：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions

        

      - 日期/时间

        | 分类     | 规则                 | 示例                                  | 示例结果                  |
        | -------- | -------------------- | ------------------------------------- | ------------------------- |
        | 日期     | @date                | @date                                 | "2013-09-05"              |
        |          | @date( format )      | @date('yyyy-MM-dd')                   | "1992-10-27"              |
        |          |                      | @date('yy-MM-dd')                     | "04-01-30"                |
        |          |                      | @date('y-MM-dd')                      | "90-07-29"                |
        |          |                      | @date('y-M-d')                        | "71-6-4"                  |
        |          |                      | @date('yyyy yy y MM M dd d')          | "1971 71 71 05 5 02 2"    |
        | 时间     | @datetime            | @datetime                             | "1988-05-25 11:34:14"     |
        |          | @datetime( format )  | @datetime('yyyy-MM-dd A HH:mm:ss')    | "1978-01-10 AM 03:59:54"  |
        |          |                      | @datetime('yy-MM-dd a HH:mm:ss')      | "92-06-22 pm 12:45:54"    |
        |          |                      | @datetime('y-MM-dd HH:mm:ss')         | "97-02-10 06:01:13"       |
        |          |                      | @datetime('y-M-d H: m:s')             | "98-10-17 18:56:12"       |
        | 当前时间 | @now                 | @now                                  |                           |
        |          | @now( unit )         | @now('year')                          | "2020-01-01 00:00:00"     |
        |          |                      | @now('month')                         | "2020-08-01 00:00:00"     |
        |          |                      | @now('week')                          | "2020-08-09 00:00:00"     |
        |          |                      | @now('day')                           | "2020-08-11 00:00:00"     |
        |          |                      | @now('hour')                          | "2020-08-11 15:00:00"     |
        |          |                      | @now('minute')                        | "2020-08-11 15:24:00"     |
        |          |                      | @now('second')                        | "2020-08-11 15:24:02"     |
        |          | @now( format )       | @now('yyyy-MM-dd HH:mm:ss SS')        | "2020-08-11 15:24:02 761" |
        |          | @now( unit, format ) | @now('day', 'yyyy-MM-dd HH:mm:ss SS') | "2020-08-11 00:00:00 000" |

        

      - 图片

        | 分类     | 规则                                                 | 示例                                             | 示例结果                                                |
        | -------- | ---------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------- |
        | 图片 URL | @image                                               | @image                                           | "http://dummyimage.com/300x250"                         |
        |          | @image( size )                                       | @image('200x100')                                | "http://dummyimage.com/200x100"                         |
        |          | @image( size, background )                           | @image('200x100', '#FF6600')                     | "http://dummyimage.com/200x100/FF6600"                  |
        |          | @image( size, background, text )                     | @image('200x100', '#4A7BF7', 'Hello')            | "http://dummyimage.com/200x100/4A7BF7&text=Hello"       |
        |          | @image( size, background, foreground, text )         | @image('200x100', '#50B347', '#FFF', 'Mock.js')  | "http://dummyimage.com/200x100/50B347/FFF&text=Mock.js" |
        |          | @image( size, background, foreground, format, text ) | @image('200x100', '#894FC4', '#FFF', 'png', '!') | "http://dummyimage.com/200x100/894FC4/FFF.png&text=!"   |
        | 图片数据 | @dataImage                                           | @dataImage                                       |                                                         |
        |          | @dataImage( size )                                   | @dataImage('200x100')                            |                                                         |
        |          | @dataImage( size, text )                             | @dataImage('200x100', 'Hello Mock.js!')          |                                                         |

        

      - 颜色

        | 规则   | 示例   | 示例结果                    |
        | ------ | ------ | --------------------------- |
        | @color | @color | "#79aaf2"                   |
        | @hex   | @hex   | "#79f2d0"                   |
        | @rgb   | @rgb   | "rgb(121, 210, 242)"        |
        | @rgba  | @rgba  | "rgba(121, 242, 167, 0.50)" |
        | @hsl   | @hsl   | "hsl(228, 82, 71)"          |

        

      - 中文文本

        | 分类 | 规则                    | 示例                                | 示例结果                                                     |
        | ---- | ----------------------- | ----------------------------------- | ------------------------------------------------------------ |
        | 段落 | @cparagraph             | @cparagraph                         | "量值这专平约称结半米化目管结北问。次便为流据由经改候究历学始影增具完。流极什其转青次各红团地真产观。争置着安眼头近效例温持完始音加照族。向素海究果存处划化复阶商手。心便近型无院些其认性几步性。" |
        |      | @cparagraph( len )      | @cparagraph(2)                      | "表验目给有新状段步表她位决华响。心什采最结小务受装通米知度是效近。" |
        |      | @cparagraph( min, max ) | @cparagraph(1, 3)                   | "东例例候专干类济带史还二任头。他千统南常公历快几要只证按规提合中。边也心近号其变代白飞小总红易。" |
        | 句子 | @csentence              | @csentence                          | "会己风感本员主见它真划生史派达原。"                         |
        |      | @csentence( len )       | @csentence(5)                       | "化而知只可。"                                               |
        |      | @csentence( min, max )  | @csentence(3, 5)                    | "称育满。"                                                   |
        | 单词 | @cword                  | @cword                              | "光"                                                         |
        |      | @cword( pool )          | @cword('零一二三四五六七八九十')    | "三"                                                         |
        |      | @cword( len )           | @cword(5)                           | "目法小"                                                     |
        |      | @cword( pool, length )  | @cword('零一二三四五六七八九十', 3) | "十一九"                                                     |
        |      | @cword( min, max )      | @cword(3, 5)                        | "除文更代"                                                   |
        | 标题 | @ctitle                 | @ctitle                             | "清照过做问"                                                 |
        |      | @ctitle( len )          | @ctitle(5)                          | "系做组平想"                                                 |
        |      | @ctitle( min, max )     | @ctitle(3, 5)                       | "热群越半"                                                   |

        

      - 中文姓名

        | 规则    | 示例    | 示例结果 |
        | ------- | ------- | -------- |
        | @cfirst | @cfirst | "赵"     |
        | @clast  | @clast  | "秀英"   |
        | @cname  | @cname  | "乔强"   |

        

      - Web 相关

        | 规则             | 示例           | 示例结果                    |
        | ---------------- | -------------- | --------------------------- |
        | @email           | @email         | "s.piqapshn@qiepsdrrm.jm"   |
        | @ip              | @ip            | "99.34.19.184"              |
        | @url             | @url           | "gopher://yux.ad/sxte"      |
        | @url( protocol ) | @url('http')   | "http://psrvbes.mobi/cxyvc" |
        | @domain          | @domain        | "hrwt.pt"                   |
        | @domain( tld )   | @domain('com') | "fdsfl.com"                 |
        | @protocol        | @protocol      | "ftp"                       |
        | @tld             | @tld           | "ga"                        |

        

      - 地址相关

        | 分类        | 规则              | 示例          | 示例结果               |
        | ----------- | ----------------- | ------------- | ---------------------- |
        | 区域        | @region           | @region       | "华北"                 |
        | 省          | @province         | @province     | "陕西省"               |
        | 市          | @city             | @city         | "淮北市"               |
        | 市 (含省)   | @city( prefix )   | @city(true)   | "广东省 肇庆市"        |
        | 区          | @county           | @county       | "东昌区"               |
        | 区 (含省市) | @county( prefix ) | @county(true) | "湖南省 怀化市 溆浦县" |
        | 邮编        | @zip              | @zip          | "843028"               |

        

      - 其他

        | 分类       | 规则                | 示例                                | 示例结果                               |
        | ---------- | ------------------- | ----------------------------------- | -------------------------------------- |
        | GUID       | @guid               | @guid                               | "D3f4c7A7-6c6B-1BEA-ff1b-DBfde6Bc1E17" |
        | 数字 ID    | @id                 | @id                                 | "450000197209231877"                   |
        | 自增 ID    | @increment          | @increment                          | "1" "2" "3"                            |
        |            | @increment( step )  | @increment(100)                     | "101" "201" "301"                      |
        | 首字母大写 | @capitalize( word ) | @capitalize('hello')                | "Hello"                                |
        | 全大写     | @upper( str )       | @upper('hello')                     | "HELLO"                                |
        | 全小写     | @lower( str )       | @lower('HELLO')                     | "hello"                                |
        | 多选一     | @pick( arr )        | @pick(["a", "e", "i", "o", "u"])    | "e"                                    |
        | 乱序       | @shuffle( arr )     | @shuffle(["a", "e", "i", "o", "u"]) | ["e","i","o","a","u"]                  |

        

      - 英文文本

        | 分类 | 规则                   | 示例             | 示例结果                                                     |
        | ---- | ---------------------- | ---------------- | ------------------------------------------------------------ |
        | 段落 | @paragraph             | @paragraph       | "Befq rkjnyqbc wrflqkzjly tlcdmjm rqpnup ipa fwxkipy bcew nbbnjfrc phroqxn fdhbqdbb cfoute icvcesoar qaetf. Rzurmrdgp wxktybadq byob oqspsewp ljxdrmhnu dwrrqyt mjwqxvrj crd nqcl hudow fokknwhqx aylfpdibdr. Qjwddj seudwxsw bugqajbik johcd bbdslyb natgh vwhbem evayqr csr ftol bjduobs cfiqy uujjldmdgo bsqq. Pttt kdwvhclmn wycb jylskicq jfleydoug gtvlqibx rekmy hmqssqiij fdsmfcs sqmktug cxqun qpecltkv dpdpiqejt lnk." |
        |      | @paragraph( len )      | @paragraph(2)    | "Qjcneih hxqtyeyhx crdori puxzq sukv mmoix gyoecfk nqwmvqlg ltvsdpshy qremm awlbgtskx qqzun wppoiasprw ldvlhgh wcyv odotfnggm bgqgcrpwu. Cjyohmm vyrj ehtnlgbg opj budsggflof nilo hlxp wwatulc hunwohq gglluimn mqht audi aepicfkv etpdld pmfuo iotsdbladi shvxfes." |
        |      | @paragraph( min, max ) | @paragraph(1, 3) | "Mgoyprc hmnscp mrhggsufd jxjpo ggbnxddtqv epdii reuwqs dtyfjmc ppq igcji muudseokx uigz oivhmvbdu csgjfslwc yhu. Mjvln pydscchcrx nnel nxmw edyi ybt kuffpq sjdocykn pxlzem jrjlm pmvck culvj xecywqm oovofkqfwu." |
        | 句子 | @sentence              | @sentence        | "Kgjo tomuhfu phqjx dhmclyl gyyrqpk hgzzrer vjlo cqcr pvkwsogqf ejngmhyuk nsod ouldhpu llkpctcnxs ubzkfu uepyxbkx kopy sdsw." |
        |      | @sentence( len )       | @sentence(5)     | "Ktlison axxvc fphbrrn txzoj jupzyrnl."                      |
        |      | @sentence( min, max )  | @sentence(3, 5)  | "Jzlop lqmkuu cjnrkhge xofrywpe."                            |
        | 单词 | @word                  | @word            | "fers"                                                       |
        |      | @word( len )           | @word(5)         | "qtpjf"                                                      |
        |      | @word( min, max )      | @word(3, 5)      | "payxj"                                                      |
        | 标题 | @title                 | @title           | "Rvugsxd Jytpw Gqrcffglc Qjt"                                |
        |      | @title( len )          | @title(5)        | "Fwbh Udrrncbdpo Xwdqhpu Zvlxyn Fvfnu"                       |
        |      | @title( min, max )     | @title(3, 5)     | "Kwnim Ekvocyml Onxnh"                                       |

        

      - 英文姓名

        | 规则   | 示例   | 示例结果           | 说明 |
        | ------ | ------ | ------------------ | ---- |
        | @first | @first | "Michelle"         |      |
        | @last  | @last  | "Lewis"            |      |
        | @name  | @name  | "Charles Williams" |      |

        

- 

- 

- 

- 