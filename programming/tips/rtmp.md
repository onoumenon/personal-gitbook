# RTMP + flv.js \(Livestreaming app\)

There are three parts to this project \(which is done as part of the [Udemy course](https://www.udemy.com/react-redux) on Redux\).

Firstly, a React \(with Redux\) client repository, which displays the UI of the application. Git repo [here](https://github.com/onoumenon/streams).

Secondly, a RESTful API repository, which hosts the streams and the user ID associated with each stream \(the user ID will be the Google UUID because we will be using Google O-Auth for authentication\). Git repo [here](https://github.com/onoumenon/streams-api).

Lastly, a Real-Time Messaging Protocol \(RTMP\) server which will host the live streams. \(To find out more about RTMP server, visit this [article](https://medium.com/@yenthanh/setup-a-rtmp-livestream-server-in-15-minutes-with-srs-1b0046c77267).\) Git repo [here](https://github.com/onoumenon/streams-rtmp).

For my project, I used node-media-server for the RTMP server, and flv.js to play the HTML5 Flash Video files \(aka live stream videos\).

### RTMP

The set up of node-media-server is fairly simple.

Simply create a new folder, `cd` into the folder, and `npm init -y` and `npm i node-media-server`.

Then visit [Node Media Server's npm page](https://github.com/illuspas/Node-Media-Server#npm-version-recommended) for the code snippet you can paste into index.js.

At the time of writing, it is

```text
const NodeMediaServer = require('node-media-server');

const config = {
  rtmp: {
    port: 1935,
    chunk_size: 60000,
    gop_cache: true,
    ping: 30,
    ping_timeout: 60
  },
  http: {
    port: 8000,
    allow_origin: '*'
  }
};

var nms = new NodeMediaServer(config)
nms.run();
```

In package.json, you can include a start script `"start": "node index.js"`.

When you run `npm start`, the RTMP server is served on port 1935 by default \(this will be where you send your live streams\), and the HTTP/ webSocket server is served on port 8000 \(where the client connects to get the live streams\).

### FLV.js

Install the `flv.js` npm package in your client.

On the [flv.js npm page](https://www.npmjs.com/package/flv.js), you will see the code snippet to get it running in "Getting Started".

```text
    if (flvjs.isSupported()) {
        var videoElement = document.getElementById('videoElement');
        var flvPlayer = flvjs.createPlayer({
            type: 'flv',
            url: 'http://example.com/flv/video.flv'
        });
        flvPlayer.attachMediaElement(videoElement);
        flvPlayer.load();
        flvPlayer.play();
    }
```

We will need to refactor that to work with our front end framework \(in this case, React\).

```text
import flv from "flv.js";

...
constructor(props) {
    super(props);
    this.videoRef = React.createRef();
  }

  componentDidMount() {
    const { id } = this.props.match.params;
    this.props.fetchStream(id);
    this.buildPlayer();
  }

  componentDidUpdate() {
    this.buildPlayer();
  }

  buildPlayer() {
    if (this.player || !this.props.stream) {
      return;
    }
    const { id } = this.props.match.params;
    this.player = flv.createPlayer({
      type: "flv",
      url: `http://localhost:8000/live/${id}.flv`
    });
    this.player.attachMediaElement(this.videoRef.current);
    this.player.load();
  }
...
```

There are a few parts to this:  
1\) After the stream is fetched, create the video element with the ref of `this.videoRef` to attach the flv player  
2\) Build the player if the player does not yet exist  
3\) If there are updates, rebuild the player.

### OBS

In order to get OBS to broadcast a livestream to our rtmp port, click on `Settings` under the `Controls` column, and create a new custom stream in `Stream` with the server `rtmp://localhost/live` and the Stream Key with the same id of the stream on your app \(eg: 1, if http://localhost:3000/streams/1\).

If successful, your app + console should look like this:

![screenshot.png](https://stuffihavelearnthome.files.wordpress.com/2019/06/screenshot.png)

For my project, I used node-media-server for the RTMP server, and flv.js to play the HTML5 Flash Video files \(aka live stream videos\).

### RTMP

The set up of node-media-server is fairly simple.

Simply create a new folder, `cd` into the folder, and `npm init -y` and `npm i node-media-server`.

Then visit [Node Media Server's npm page](https://github.com/illuspas/Node-Media-Server#npm-version-recommended) for the code snippet you can paste into index.js.

At the time of writing, it is

```text
const NodeMediaServer = require('node-media-server');

const config = {
  rtmp: {
    port: 1935,
    chunk_size: 60000,
    gop_cache: true,
    ping: 30,
    ping_timeout: 60
  },
  http: {
    port: 8000,
    allow_origin: '*'
  }
};

var nms = new NodeMediaServer(config)
nms.run();
```

In package.json, you can include a start script `"start": "node index.js"`.

When you run `npm start`, the RTMP server is served on port 1935 by default \(this will be where you send your live streams\), and the HTTP/ webSocket server is served on port 8000 \(where the client connects to get the live streams\).

### FLV.js

Install the `flv.js` npm package in your client.

On the [flv.js npm page](https://www.npmjs.com/package/flv.js), you will see the code snippet to get it running in "Getting Started".

```text
    if (flvjs.isSupported()) {
        var videoElement = document.getElementById('videoElement');
        var flvPlayer = flvjs.createPlayer({
            type: 'flv',
            url: 'http://example.com/flv/video.flv'
        });
        flvPlayer.attachMediaElement(videoElement);
        flvPlayer.load();
        flvPlayer.play();
    }
```

We will need to refactor that to work with our front end framework \(in this case, React\).

```text
import flv from "flv.js";

...
constructor(props) {
    super(props);
    this.videoRef = React.createRef();
  }

  componentDidMount() {
    const { id } = this.props.match.params;
    this.props.fetchStream(id);
    this.buildPlayer();
  }

  componentDidUpdate() {
    this.buildPlayer();
  }

  buildPlayer() {
    if (this.player || !this.props.stream) {
      return;
    }
    const { id } = this.props.match.params;
    this.player = flv.createPlayer({
      type: "flv",
      url: `http://localhost:8000/live/${id}.flv`
    });
    this.player.attachMediaElement(this.videoRef.current);
    this.player.load();
  }
...
```

There are a few parts to this:  
1\) After the stream is fetched, create the video element with the ref of `this.videoRef` to attach the flv player  
2\) Build the player if the player does not yet exist  
3\) If there are updates, rebuild the player.

### OBS

In order to get OBS to broadcast a livestream to our rtmp port, click on `Settings` under the `Controls` column, and create a new custom stream in `Stream` with the server `rtmp://localhost/live` and the Stream Key with the same id of the stream on your app \(eg: 1, if http://localhost:3000/streams/1\).

If successful, your app + console should look like this:

![screenshot.png](https://stuffihavelearnthome.files.wordpress.com/2019/06/screenshot.png)

