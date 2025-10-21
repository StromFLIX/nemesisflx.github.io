# nemesisflx.github.io

TThis project was migrated from Nuxt 2 (Vue 2) to Nuxt 3 (Vue 3). Key changes include:

- Updated to Vue 3 with Composition API
- FontAwesome updated to use `@fortawesome/vue-fontawesome` v3
- Tailwind CSS updated to v3 compatible version
- Build output now in `.output/public/` instead of `dist/`

## Special Directories

### `pages`

This directory contains your application views and routes. Nuxt will read all the `*.vue` files inside this directory and setup Vue Router automatically.s been migrated from Nuxt 2 (Vue 2) to Nuxt 3 (Vue 3).

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run preview

# generate static project
$ npm run generate
```

For detailed explanation on how things work, check out the [Nuxt 3 documentation](https://nuxt.com/docs).

## Migration Notes

This project was migrated from Nuxt 2 to Nuxt 3 following the official migration guide. Key changes include:

- Updated to Vue 3 with Composition API
- Pages moved to `app/pages/` directory
- Static files moved from `static/` to `public/`
- FontAwesome updated to use `@fortawesome/vue-fontawesome` v3
- Tailwind CSS updated to v3 compatible version
- Build output now in `.output/public/` instead of `dist/`

## Special Directories

### `app/pages`

This directory contains your application views and routes. Nuxt will read all the `*.vue` files inside this directory and setup Vue Router automatically.

More information: [Nuxt 3 Pages Documentation](https://nuxt.com/docs/guide/directory-structure/pages)

### `components`

The components directory contains your Vue.js components. Components are automatically imported in Nuxt 3.

More information: [Nuxt 3 Components Documentation](https://nuxt.com/docs/guide/directory-structure/components)

### `plugins`

The plugins directory contains JavaScript plugins that you want to run before instantiating the root Vue.js Application.

More information: [Nuxt 3 Plugins Documentation](https://nuxt.com/docs/guide/directory-structure/plugins)

### `public`

This directory contains your static files. Each file inside this directory is mapped to `/`.

More information: [Nuxt 3 Public Directory Documentation](https://nuxt.com/docs/guide/directory-structure/public)
