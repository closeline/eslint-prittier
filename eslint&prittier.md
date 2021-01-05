## ##1. ESLint と Prettier の CI 環境を構築

###1-1. パッケージのインストール
`npm install --save-dev eslint eslint-config-prettier prettier @typescript-eslint/parser @typescript-eslint/eslint-plugin husky lint-staged`

###1-2.prittier の設定
.pritterrc を作成
‘{
"printWidth": 120,
"singleQuote": true,
"semi": false
}’

###1-3.eslint の設定
.eslintrc.js 　　を作成
`module.exports = { env: { browser: true, es6: true }, extends: [ "eslint:recommended", "plugin:@typescript-eslint/recommended", // TypeScriptでチェックされる項目をLintから除外する設定 "prettier", // prettierのextendsは他のextendsより後に記述する "prettier/@typescript-eslint", ], plugins: ["@typescript-eslint"], parser: "@typescript-eslint/parser", parserOptions: { "sourceType": "module", "project": "./tsconfig.json" // TypeScriptのLint時に参照するconfigファイルを指定 }, root: true, // 上位ディレクトリにある他のeslintrcを参照しないようにする rules: {} }`

### 1-4.package.json に以下を追加

`"scripts": { "build": "webpack --mode=production", "start": "webpack-cli serve --mode development", *"lint": "eslint --fix 'src/**/*.{js,ts}'",*`

一番下に以下を記載
` "husky": { "hooks": { "pre-commit": "lint-staged" } }, "lint-staged": { "src/**/*.{js,jsx,ts,tsx}": [ "npm run lint-fix" ] }`

### 1-5 git 実行

git init
git add .
git commit -m “”

### 1-6.eslint/prittier の実行

npm run lint-fix

###番外. husky がもし動かなかったら...
.git/hooks が正常に作成されていない可能性アリ これで確認する
`ls -la .git/hooks/ls -la .git/hooks/`
.sample しかなかったら NG
NG の場合はインストールし直す
`npm uninstall huksy`
`npm install --save-dev husky`
もう一度 hooks を確認
`ls -la .git/hooks/ls -la .git/hooks/`
