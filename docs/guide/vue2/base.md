# Webpack 4 vue.config.js 基础配置

如果你的 `Vue Router` 模式是 hash

```javascript
publicPath: './',
```

如果你的 `Vue Router` 模式是 history 这里的 publicPath 和你的 `Vue Router` `base` **保持一致**

```javascript
publicPath: '/app/',
```

配置如下

```javascript
import { createVitePlugins } from './config/vite/plugins';
import { resolve } from 'path';
import { ConfigEnv, UserConfigExport } from 'vite';

const pathResolve = (dir: string) => {
  return resolve(process.cwd(), '.', dir);
};

// https://vitejs.dev/config/
export default function ({ command }: ConfigEnv): UserConfigExport {
  const isProduction = command === 'build';
  const root = process.cwd();
  return {
    root,
    resolve: {
      alias: [
        {
          find: 'vue-i18n',
          replacement: 'vue-i18n/dist/vue-i18n.cjs.js',
        },
        // /@/xxxx => src/xxxx
        {
          find: /\/@\//,
          replacement: pathResolve('src') + '/',
        },
        // /#/xxxx => types/xxxx
        {
          find: /\/#\//,
          replacement: pathResolve('types') + '/',
        },
      ],
    },
    server: {
      host: true,
      hmr: true,
    },
    plugins: createVitePlugins(isProduction),
    css: {
      preprocessorOptions: {
        scss: {
          // 配置 nutui 全局 scss 变量
          additionalData: `@import "@nutui/nutui/dist/styles/variables.scss";@import '/@/styles/mixin.scss';`,
        },
      },
    },
  };
}
```
