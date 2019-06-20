# Apple Music Web Player

[music.zacharyseguin.ca](https://music.zacharyseguin.ca)

This is a web player for Apple Music using [MusicKit JS](https://developer.apple.com/documentation/musickitjs).

> Apple and Apple Music are trademarks of Apple Inc., registered in the U.S. and other countries.

## Features

\* denotes features only available when signed into Apple Music.

- Access to the entire Apple Music library
- Access to your personal Music Library*

### Playback

- Full track playback*
    - Control volume, shuffle and repeat modes
- Add songs to the playback queue
- Full screen player
- Use keyboard media controls to play/pause, skip track and return to previous track (browser must support Media Session)
- Notifications when the track changes (can be disabled if not desired)

### Discovery

- Top charts
- For you*
- Recent*
- Search both Apple Music and your Library*

### Library management

- Add playlists, albums and songs to your library*
- Add songs to playlists (new or existing)*
- Love and dislike songs*

## Screenshots

![Screenshot: Playlist](promo/screenshot-playlist.png)

![Screenshot: Top charts](promo/screenshot-top-charts.png)

![Screenshot: Full screen player](promo/screenshot-full-screen-player.png)

---

## Development

This web player is written using [Vue.js](https://vuejs.org).

### Getting started

```sh
git clone https://github.com/zachomedia/apple-music-webplayer.git
cd apple-music-webplayer

# Install dependencies
npm install

# Add the private settings
cp src/private.js.sample src/private.js

# Add your Developer Token to src/private.js
#  To generate one, see below.

# To run a local development instance
npm run serve

# To build the app (to dist folder)
npm run build
```

### Generating an Apple Music Developer token

[Apple's official documentation](https://developer.apple.com/documentation/applemusicapi/getting_keys_and_creating_tokens) provides a base, and then you can visit [Creating an Apple Music API Token](https://medium.com/@leemartin/creating-an-apple-music-api-token-e0e5067e4281) for a great guide on how to generate the token.


### 项目部署到nginx

- 安装和编译

```$xslt

# 首先安装依赖
npm install

# 编译app 生成dist 目录
npm run build

```

- 上传到服务器
build之后将`dist`文件夹上传到服务器中，我是上传到linux的`/var/www`下新建的一个目录下

- 配置nginx

```markdown
 server {
            listen       80;
            server_name  localhost;

            #charset koi8-r;

	    access_log /var/log/nginx/applemusic_access.log;
   	    error_log /var/log/nginx/applemusic_error.log;	
           
            location /static {
          	expires max;
          	alias /var/www/applemusic/dist/static;
            }
            location / {
                root   /var/www/applemusic/dist;
                index  index.html index.htm;
	    }
}

```

- 在浏览器上访问，通过nginx中server配置的端口访问。端口为server上配的listen端口



### 注意事项

- 1.项目在本地run serve 情况下显示正常，在build后再运行dist，显示font-awesome的css文件依赖的几个图标文件引用路径报错。
比如这个路径，就是错误的路径：`http://localhost:63342/apple-music-webplayer/dist/static/css/static/fonts/fontawesome-webfont.af7ae50.woff2

解决方法：
将配置中option注释掉即可解决路径问题 参考https://www.jb51.net/article/146684.htm

- 2.本地开发运行使用`npm run serve`，上下部署使用`npm run build`然后上传dist到服务器

- 3.`assetsPublicPath`在`index.js`中正确的配置如下：
```$xslt
module.exports = {
  dev: {

    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
  },

  build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: './',
}
```
