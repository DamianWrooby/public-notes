---
id: 9sURuAoFH3eDnAC0WaMUp
title: Webpack
desc: ''
updated: 1632132473785
created: 1632132371510
---

## Podstawowa konfiguracja

html-webpack-plugin - plugin do tworzenia index.html
clean-webpack-plugin - plugin do sprzątania przy każdym buildzie

```javascript
// const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");
// const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = {
	mode: "development",
	devtool: "inline-source-map",
	devServer: {
		static: "./dist",
	},
	entry: "./src/app.js",
	output: {
		path: path.resolve(__dirname, "./dist"),
		filename: "bundle.js",
	},
	plugins: [
		// new HtmlWebpackPlugin({
		// 	title: "Quiz",
		// }),
		// new CleanWebpackPlugin(),
	],
	module: {
		rules: [
			{
				test: /\.js$/,
				exclude: /node_modules/,
				use: {
					loader: "babel-loader",
					options: {
						presets: ["@babel/preset-env"],
					},
				},
			},
			{
				test: /\.s[ac]ss$/i,
				use: [
					"style-loader",
					"css-loader",
					"sass-loader",
				],
			},
		],
	},
};

```
