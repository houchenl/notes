
# BuildConfig

每个模块都有一个独立的BuildConfig, APPLICATION_ID为该模块的包名。

父模块指定依赖的子模块时，需要指定编译类型(BuildType)
releaseCompile project(path: ':common', configuration: 'release')
debugCompile project(path: ':common', configuration: 'debug')

子模块需要在android下添加publishNonDefault true，以与之对应
