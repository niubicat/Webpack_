[*webpack配置上篇*](http://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651226691&idx=1&sn=732f8a5e611437eb8c8bef420b9ccd6e&chksm=bd495bc78a3ed2d19ca55e1a3dfe137a50a0ffe32003a2231d471239faa47a7bf4f5c1121871&mpshare=1&scene=23&srcid=07116Qq8RNRhUjocDLzdGzD2#rd)
[*webpack配置下篇*](http://mp.weixin.qq.com/s?__biz=MjM5MTA1MjAxMQ==&mid=2651226691&idx=2&sn=1b3c920e37eaf5facf73a81d16c5a5d4&chksm=bd495bc78a3ed2d14102c61d103ce1d361528081e5629f8cfe498a697d24621c70ed4877544c&mpshare=1&scene=23&srcid=0711JmMfWhs8dG1NnFjoRo0D#rd)

### webpack 2 打包实战

#### 1、安装Node.js
    webpack是基于我大Node.js的打包工具, 上来第一件事自然是先安装Node.js了

#### 2、初始化一个项目
    打开命令行窗口, cd到刚才建的simple目录. 然后执行这个命令初始化项目:
    ├── dist                      打包输出目录, 只需部署这个目录到生产环境
    ├── package.json              项目配置信息
    ├── node_modules              npm安装的依赖包都在这里面
    ├── src                       我们的源代码
    │   ├── components            可以复用的模块放在这里面
    │   ├── index.html            入口html
    │   ├── index.js              入口js
    │   ├── libs                  不在npm和git上的库扔这里
    │   └── views                 页面放这里
    └── webpack.config.js         webpack 配置文件

    执行这个命令初始化项目:
    npm init
    生成一个默认的项目配置文件package.json

#### 3、给项目加上语法报错和代码规范检查
    npm install eslint eslint-config-enough eslint-loader --save-dev
    取package.json中的devDependencies和dependencies字段, 把记录的包的相应版本下载下来
    npm install

    * eslint-config-enough是配置文件, 它规定了代码规范, 要使它生效
    * eslint-loader用于在webpack编译的时候检查代码, 如果有错误, webpack会报错
    
#### 4、WebStorm需要在设置中打开eslint 开关
    业界最有名的语法规范是airbnb出品的, 但它规定的太死板了, 比如不允许使用for-of和for-in等

#### 5、安装webpack和Babel
    npm install webpack webpack-dev-server html-webpack-plugin html-loader css-loader style-loader file-loader url-loader --save-dev

    * webpack-dev-server是webpack提供的用来开发调试的服务器, 让你可以用 http://127.0.0.1:8080/ 这样的url打开页面来调试, 有了它就不用配置nginx了, 方便很多.
    * html-webpack-plugin, html-loader,css-loader,style-loader等看名字就知道是打包html文件, css文件的插件
    * file-loader和url-loader是打包二进制文件的插件

#### 6、让不支持ES6的浏览器(比如IE)也能照常运行, 我们需要安装babel
    npm install babel-core babel-preset-env babel-loader --save-dev
    npm install babel-preset-es2015 --save-dev

    * babel-core顾名思义是babel的核心编译器.babel-preset-env是一个配置文件, 我们可以使用这个配置文件转换ES2015/ES2016/ES2017到ES5, 是的, 不只ES6哦. babel还有其他配置文件. 如果只想用ES6, 可以安装babel-preset-es2015
    * babel-loader是webpack的插件

#### 7、配置webpack
    配置文件webpack.config.js, 注意这个文件是在node.js中运行的, 因此不支持ES6的import语法
    运行开发环境用的webpack-dev-server
    ./node_modules/.bin/webpack-dev-server -d --hot
    上面的命令适用于Mac/Linux等*nix系统,
    也适用于Windows上的PowerShell和bash/zsh环境(Bash on Wbuntu on Windows, Git Bash,Babun,MSYS2等).
    node_modules\.bin\webpack-dev-server -d --hot
    Windows同学使用Bash on Ubuntu on Windows, 可以避免很多跨平台的问题, 比如设置环境变量.
    npm会把包的可执行文件安装到./node_modules/.bin/目录下, 所以我们要在这个目录下执行命令.

    * -d参数是开发环境(Development)的意思, 它会在我们的配置文件中插入调试相关的选项,
    比如打开debug, 打开sourceMap, 代码中插入源文件路径注释.
    * --hot开启热更新功能, 参数会帮我们往配置里添加HotModuleReplacementPlugin插件,
    虽然可以在配置里自己写, 但有点麻烦, 用命令行参数方便很多

    开发环境编译试过之后, 我们试试看编译生产环境的代码:
    ./node_modules/.bin/webpack -p
    -p参数会开启生产环境模式, 这个模式下webpack会将代码做压缩等优化

    我们可以利用npm的特性, 把命令写在package.json 中
    package.json中的scripts对象, 可以用来写一些脚本命令,
    命令不需要前缀目录 ./node_modules/.bin/, npm会自动寻找该目录下的命令

    npm run dev 来启动开发环境
    npm run build 打包生产环境的代码

#### 8、进阶配置

    * 指定静态资源的url路径前缀

    * 各个页面分开打包

    * 打包时区分开发环境和生产环境

    * 输出的entry文件加上hash

    * 第三方库和业务代码分开打包

    * 开发环境关闭performance.hints

    * 配置favicon

    * 开发环境允许其他电脑访问

    * 打包时自定义部分参数

    * webpack-dev-server处理带后缀名的文件的特殊规则

    * 代码中插入环境变量

    * 简化import路径

    * 优化babel编译后的代码性能

    * 使用webpack 2自带的ES6模块处理功能

    * 使用autoprefixer自动创建css的vendor prefixes

    * 编译前清空dist目录

    * 那么, 让我们在上面的配置的基础上继续完善, 下面的代码我们只写出改变的部分. 代码在examples/advanced目录.

#### 9、指定静态资源的url路径前缀
    我们的资源文件的url直接在根目录, 比如http://127.0.0.1:8100/index.js, 这样做缓存控制和CDN都不方便, 我们需要给资源文件的url加一个前缀, 比如http://127.0.0.1:8100/assets/index.js这样

#### 10、各个页面分开打包
    浏览器只需加载当前访问的页面的代码
    webpack可以使用异步加载文件的方式引用模块, webpack 1的API是require.ensure(), webpack 2开始支持TC39的dynamic import. 我们这里就使用新的import()来实现页面分开打包异步加载

    load(path) {
        import('./views' + path + '/index.js').then(module => {
            // export default ... 的内容通过module.default访问
            const View = module.default
            const view = new View()
            view.mount(document.body)
        })
    }

    这样我们就不需要在开头把所有页面文件都import进来了

    因为import()还没有正式进入标准, 因此babel和eslint需要插件来支持它
    npm install babel-eslint babel-preset-stage-2 --save-dev

#### 11、打包时区分开发环境和生产环境
    如果webpack.config.js导出的是一个function, 那么webpack会执行它, 并把返回的结果作为配置对象

    module.exports = (options = {}) => {
        return {
            // 配置内容
        }
    }

    该function接受一个参数, 这个参数的值是由命令行传入的. 比如当我们在命令行中执行:
    webpack --env.dev --env.server localhost
    那么options值为 `{ dev: true, server: 'localhost' }`

    该参数对 webpack-dev-server 命令同样有效

#### 12、输出的entry文件加上hash
    chunkFilename可以加上[chunkhash]防止浏览器读取错误缓存, 那么entry同样需要加上hash
    但使用webpack-dev-server启动开发环境时, entry文件是没有[chunkhash]的, 用了会报错. 因此我们需要利用上面提到的区分开发环境和生产环境的功能, 只在打包生产环境代码时加上[chunkhash]

    有人可能注意到官网文档中还有一个[hash]占位符, 这个hash是整个编译过程产生的一个总的hash值, 而不是单个文件的hash值, 项目中任何一个文件的改动, 都会造成这个hash值的改变. [hash]占位符是始终存在的, 但我们不希望修改一个文件导致所有输出的文件hash都改变, 这样就无法利用浏览器缓存了. 因此这个[hash]意义不大

#### 13、第三方库和业务代码分开打包
    这样更新业务代码时可以借助浏览器缓存, 用户不需要重新下载没有发生变化的第三方库

    入口的html文件引两个js, vendor.js和index.js。vendor.js用来引用第三方库, 
    比如这儿我们引入一个第三方库来做路由
    npm install spa-history --save
    然后在vendor.js中, 我们引用一下它
    import 'spa-history/PathHistory'
    我们import它但不需要做什么, 这样webpack打包的时候会把这个第三方库打包进vendor.js

    然后在src/index.js中, 我们使用它:
    import PathHistory from 'spa-history/PathHistory'

    const history = new PathHistory({
    change(location) {
        // 使用import()将加载的js文件分开打包, 这样实现了仅加载访问的页面
        import('./views' + location.path + '/index.js').then(module => {
        // export default ... 的内容通过module.default访问
        const View = module.default
        const view = new View()
        view.mount(document.body)
        })
    }
    })

    history.hookAnchorElements()
    history.start()

    页面foo和bar的js和html文件因为路由的改变也要做些微调

    src/views/foo/index.js

    import template from './index.html'
    import './style.css'

    export default class {
        mount(container) {
            document.title = 'foo'
            container.innerHTML = template
        }
    }

    src/views/foo/index.html

    <div class="foo">
        <h1>Page Foo</h1>
        <a href="/bar">goto bar</a>

        <p>
            <img src="smallpic.png">
        </p>

        <p>
            <img src="/views/foo/largepic.png">
        </p>
    </div>

    src/views/bar/index.js

    import template from './index.html'
    import './style.css'

    export default class {
    mount(container) {
        document.title = 'bar'
        container.innerHTML = template
    }

    src/views/bar/index.html

    <div class="bar">
        <h1>Page Bar</h1>
        <a href="/foo">goto foo</a>
    </div>

#### 14、开发环境关闭performance.hints
    我们注意到运行开发环境是命令行会报一段warning:
    WARNING in asset size limit: The following asset(s) exceed the recommended size limit (250 kB).
    This can impact web performance.

    这是说建议每个输出的js文件的大小不要超过250k. 但开发环境因为包含了sourcemap并且代码未压缩所以一般都会超过这个大小, 所以我们可以在开发环境把这个warning关闭.

    webpack配置中加入:
    {
        performance: {
            hints: options.dev ? false : 'warning'
        }
    }

#### 15、配置favicon
    在src目录中放一张favicon.png, 然后src/index.html的<head> 中插入
    <link rel="icon" type="image/png" href="favicon.png">

    修改webpack 配置

    其实html-webpack-plugin接受一个favicon参数, 可以指定favicon文件路径, 会自动打包插入到html文件中. 但它有个bug, 打包后的文件名路径不带hash, 就算有hash, 它也是[hash], 而不是[chunkhash], 导致修改代码也会改变favicon打包输出的文件名. issue中提到的favicons-webpack-plugin倒是可以用, 但它依赖PhantomJS, 非常大.

#### 16、开发环境允许其他电脑访问
    webpack配置devServer.host为`0.0.0.0`即可.

#### 17、打包时自定义部分参数
    在多人开发时, 每个人可能需要有自己的配置, 比如说webpack-dev-server监听的端口号, 如果写死在webpack配置里, 而那个端口号在某个同学的电脑上被其他进程占用了, 简单粗暴的修改webpack.config.js会导致提交代码后其他同学的端口也被改掉.

    还有一点就是开发环境/测试环境/生产环境的部分webpack配置是不同的, 比如publicPath在生产环境可能要配置一个CDN地址.

    我们在根目录建立一个文件夹config, 里面创建3个配置文件:
    default.js: 生产环境 
    dev.js: 默认开发环境
    local.js: 个人本地环境, 在dev.js基础上修改部分参数 .

    package.json修改scripts

    这里的关键是npm run传进来的自定义参数可以通过process.env.npm_config_*获得. 参数中如果有-会被转成_
    --env.*传进来的参数可以通过options.*获得. 我们优先使用npm run指定的配置文件. 这样我们可以在命令行覆盖scripts中指定的配置文件:
    npm run dev --config=CONFIG_NAME
    local命令就是这样做的.

    这样, 当我们执行npm run dev时使用的是dev.js, 执行npm run local使用local.js, 执行npm run build使用default.js.

    重点::

    config.devServer.proxy用来配置后端api的反向代理, ajax /api/auth/*的请求会被转发到http://api.example.dev/auth/*, /api/pay/*的请求会被转发到http://api.example.dev/pay/*.

    changeOrigin会修改HTTP请求头中的Host为target的域名, 这里会被改为api.example.dev

    pathRewrite用来改写URL, 这里我们把/api前缀去掉.

    还有一点, 我们不需要把自己个人用的配置文件提交到git, 所以我们在.gitignore 中加入:
    config/*
    !config/default.js
    !config/dev.js
    把config目录排除掉, 但是保留生产环境和dev默认配置文件.

#### 18、webpack-dev-server处理带后缀名的文件的特殊规则
    当处理带后缀名的请求时, 比如 http://localhost:8100/bar.do , webpack-dev-server会认为它应该是一个实际存在的文件, 就算找不到该文件, 也不会fallback到index.html, 而是返回404. 但在SPA应用中这不是我们希望的. 幸好webpack-dev-server有一个配置选项disableDotRule: true可以禁用这个规则, 使带后缀的文件当不存在时也能fallback到index.html

#### 19、代码中插入环境变量
    在业务代码中, 有些变量在开发环境和生产环境是不同的, 比如域名, 后台API地址等. 还有开发环境可能需要打印调试信息等.
    可以使用DefinePlugin插件在打包时往代码中插入需要的环境变量

    DefinePlugin插件的原理很简单, 如果我们在代码中写:

    console.log('Debug');

    它会做类似这样的处理：
    'console.log(DEBUG)'.replace('DEBUG', true)
    最后生成:
    console.log(true)
    这里有一点需要注意, 像这里的VERSION, 如果我们不对pkgInfo.version做JSON.stringify(),
    console.log(VERSION)
    然后做替换操作:
    'console.log(VERSION)'.replace('VERSION', '1.0.0')
    最后生成:
    console.log(1.0.0)
    这样语法就错误了. 所以, 我们需要JSON.stringify(pkgInfo.version)转一下变成'"1.0.0"', 替换的时候才会带引号.

    还有一点, webpack打包压缩的时候, 会把代码进行优化, 比如:
    if (DEBUG) {
    console.log('debug mode')
    } else {
    console.log('production mode')
    }

    会被编译成:
    if (false) {
    console.log('debug mode')
    } else {
    console.log('production mode')
    }

    然后压缩优化为:
    console.log('production mode')

#### 20、简化import路径
    文件a引入文件b时, b的路径是相对于a文件所在目录的. 如果a和b在不同的目录, 藏得又深, 写起来就会很麻烦:
    import b from '../../../components/b'

    为了方便, 我们可以定义一个路径别名(alias):
    resolve: {
        alias: {
            '~': resolve(__dirname, 'src')
        }
    }

    这样, 我们可以以`src`目录为基础路径来`import`文件:
    import b from '~/components/b'

    html中的<img>标签没法使用这个别名功能, 但html-loader有一个root参数, 可以使 / 开头的文件相对于root目录解析.

    那么, <img src="/favicon.png">就能顺利指向到src目录下的favicon.png文件, 不需要关心当前文件和目标文件的相对路径.

    PS: 在调试<img>标签的时候遇到一个坑, html-loader会解析<!-- -->注释中的内容, 之前在注释中写的
    如： 
    <!--
    大于10kb的图片, 图片会被储存到输出目录, src会被替换为打包后的路径
    <img src="/assets/f78661bef717cf2cc2c2e5158f196384.png">
    -->
    之前因为没有加root参数, 所以`/`开头的文件名不会被解析, 加了root导致编译时报错, 找不到该文件. 大家记住这一点.

#### 21、优化babel编译后的代码性能
    babel编译后的代码一般会造成性能损失, babel提供了一个loose选项, 使编译后的代码不需要完全遵循ES6规定, 简化编译后的代码, 提高代码执行效率:
    修改package.json
    {
        "babel": {
            "presets": [
            [
                "env",
                {
                "loose": true
                }
            ],
            "stage-2"
            ]
        }
    }

    重点::
    但这么做会有兼容性的风险, 可能会导致ES6源码理应的执行结果和编译后的ES5代码的实际结果并不一致. 如果代码没有遇到实际的效率瓶颈, 官方不建议使用loose模式.

#### 22、使用webpack 2自带的ES6模块处理功能
    我们目前的配置, babel会把ES6模块定义转为CommonJS定义, 但webpack自己可以处理import和export, 而且webpack处理import时会做代码优化, 把没用到的部分代码删除掉. 因此我们通过babel提供的modules: false选项把ES6模块转为CommonJS模块的功能给关闭掉.
    {
        "babel": {
            "presets": [
            [
                "env",
                {
                "loose": true,
                "modules": false
                }
            ],
            "stage-2"
            ]
        }
    }

#### 23、使用autoprefixer自动创建css的vendor prefixes
    css有一个很麻烦的问题就是比较新的css属性在各个浏览器里是要加前缀的, 我们可以使用autoprefixer工具自动创建这些浏览器规则。
    npm install postcss-loader autoprefixer --save-dev

    autoprefixer是postcss的一个插件, 所以我们也要安装postcss的webpack loader.
    修改一下webpack的css rule:
    {
        test: /\.css$/,
        use: ['style-loader', 'css-loader', 'postcss-loader']
    }

#### 24、编译前清空dist目录
    不清空的话上次编译生成的文件会遗留在dist目录中, 我们最好先把目录清空一下. macOS/Linux下可以用rm -rf dist搞定, 考虑到跨平台的需求, 我们可以用rimraf:
    npm install rimraf --save-dev
    package.json修改一下:
    "build": "rimraf dist && webpack -p --env.config production"

#### 25、传统的多页面网站(MPA)能否用webpack打包?
    对于多页面网站, 我们最多的是用Grunt或Gulp来打包, 因为这种简单的页面对模块化编程的需求不高. 但如果你喜欢上使用import来引入库, 那么我们仍然可以使用webpack来打包.

    MPA意味着并没不是一个单一的html入口和js入口, 而是每个页面对应一个html和多个js. 那么我们可以把项目结构设计为:

    ├── dist
    ├── package.json
    ├── node_modules
    ├── src
    │   ├── components
    │   ├── libs
    |   ├── favicon.png
    |   ├── vendor.js             所有页面公用的第三方库
    │   └── pages                 页面放这里
    |       ├── foo               编译后生成 http://localhost:8100/foo.html
    |       |    ├── index.html
    |       |    ├── index.js
    |       |    ├── style.css
    |       |    └── pic.png
    |       └── bar               http://localhost:8100/bar.html
    |           ├── index.html
    |           ├── index.js
    |           ├── style.css
    |           └── baz           http://localhost:8100/bar/baz.html
    |               ├── index.html
    |               ├── index.js
    |               └── style.css
    └── webpack.config.js

    这里每个页面的index.html是个完整的从<!DOCTYPE html>开头到</html>结束的页面, 这些文件都要用html-webpack-plugin处理. index.js是每个页面的业务逻辑, 全部作为入口js配置到entry中. 页面公用的第三方库仍然打包进vendor.js. 这里我们需要用glob库来把这些文件都筛选出来批量操作 .

    npm install glob --save-dev
    webpack.config.js修改的地方:

    // ...
    const glob = require('glob')

    module.exports = (options = {}) => {
        // ...

        const entries = glob.sync('./src/**/index.js')
        const entryJsList = {}
        const entryHtmlList = []
        for (const path of entries) {
            const chunkName = path.slice('./src/pages/'.length, -'/index.js'.length)
            entryJsList[chunkName] = path
            entryHtmlList.push(new HtmlWebpackPlugin({
            template: path.replace('index.js', 'index.html'),
            filename: chunkName + '.html',
            chunks: ['manifest', 'vendor', chunkName]
            }))
        }

        return {
            entry: Object.assign({
            vendor: './src/vendor'
            }, entryJsList),

            // ...

            plugins: [
            ...entryHtmlList,
            // ...
            ]
        }
    }

#### 26、为什么不使用webpack.config.babel.js
    部分同学可能知道webpack可以读取webpack.config.babel.js, 它会先调用babel将文件编译后再执行. 但这里有两个坑:
    1. 由于我们的package.json中的babel配置指定了modules: false, 所以babel并不会转码import, 这导致编译后的webpack配置文件仍然无法在node.js中执行, 解决方案是package.json不指定modules: false, 而在babel-loader中的options中配置babel. 这样webpack.config.babel.js会使用package.json的babel配置编译, 而webpack编译的js会使用babel-loader指定的配置编译.

    {
        test: /\.js$/,
        exclude: /node_modules/,
        use: [
            {
            loader: 'babel-loader',
            options: {
                presets: [
                ['env', {
                    loose: true,
                    modules: false
                }],
                'stage-2'
                ]
            }
            },

            'eslint-loader'
        ]
    }

    2. postcss的配置不支持先用babel转码, 这导致了我们的配置文件格式的不统一.
    
    综上, 还是只在src目录中的文件使用ES6模块规范会比较方便一点.


        
