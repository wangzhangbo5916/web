# canvas和多媒体

## audio

Audio API 允许你在网页上播放、控制和操作音频文件。通过结合 HTML5 的 `<audio>` 元素和 JavaScript，你可以创建强大的音频播放器，控制音频的播放、暂停、音量、进度等。

### 基本使用

首先，在 HTML 中添加一个 `<audio>` 元素，并提供音频文件的路径。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Audio Example</title>
</head>
<body>
  <h1>Audio Example</h1>
  <audio id="myAudio" controls>
    <source src="path/to/audio.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>
  <script src="script.js"></script>
</body>
</html>
```

你可以通过 JavaScript 获取 `<audio>` 元素，并使用其方法和属性来控制音频播放。

```js
const audio = document.getElementById('myAudio');

// 播放音频
audio.play();

// 暂停音频
audio.pause();

// 设置音量（范围 0.0 到 1.0）
audio.volume = 0.5;

// 跳转到指定时间（秒）
audio.currentTime = 30;
```

### 音频控制示例

以下是一个示例，展示了如何使用 JavaScript 创建一个简单的音频播放器，包含播放、暂停、音量控制和进度条。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Audio Player</title>
  <style>
    .controls {
      display: flex;
      align-items: center;
    }
    button {
      margin-right: 10px;
    }
    input[type="range"] {
      margin-right: 10px;
    }
  </style>
</head>
<body>
  <h1>Audio Player</h1>
  <audio id="myAudio">
    <source src="path/to/audio.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>
  <div class="controls">
    <button id="playButton">Play</button>
    <button id="pauseButton">Pause</button>
    <input type="range" id="volumeSlider" min="0" max="1" step="0.01" value="0.5">
    <input type="range" id="progressSlider" min="0" max="100" step="1" value="0">
  </div>
  <script src="script.js"></script>
</body>
</html>
```

```js
const audio = document.getElementById('myAudio');
const playButton = document.getElementById('playButton');
const pauseButton = document.getElementById('pauseButton');
const volumeSlider = document.getElementById('volumeSlider');
const progressSlider = document.getElementById('progressSlider');

// 播放音频
playButton.addEventListener('click', () => {
  audio.play();
});

// 暂停音频
pauseButton.addEventListener('click', () => {
  audio.pause();
});

// 调整音量
volumeSlider.addEventListener('input', (e) => {
  audio.volume = e.target.value;
});

// 更新进度条
audio.addEventListener('timeupdate', () => {
  const progress = (audio.currentTime / audio.duration) * 100;
  progressSlider.value = progress;
});

// 跳转到指定时间
progressSlider.addEventListener('input', (e) => {
  const seekTime = (e.target.value / 100) * audio.duration;
  audio.currentTime = seekTime;
});
```

### 高级功能

#### 1. 处理音频事件

你可以监听音频元素的各种事件，如播放、暂停、结束、时间更新等，以便在这些事件发生时执行特定的操作。

```js
audio.addEventListener('play', () => {
  console.log('Audio is playing');
});

audio.addEventListener('pause', () => {
  console.log('Audio is paused');
});

audio.addEventListener('ended', () => {
  console.log('Audio has ended');
});

audio.addEventListener('timeupdate', () => {
  console.log(`Current time: ${audio.currentTime}`);
});
```

#### 2. 使用 Audio 对象

除了使用 `<audio>` 元素，你还可以直接使用 JavaScript 的 Audio 对象来创建和控制音频。

```js
const audio = new Audio('path/to/audio.mp3');

// 播放音频
audio.play();

// 暂停音频
audio.pause();
```

#### 3. 音频分析

使用 Web Audio API，你可以进行更高级的音频处理和分析，例如创建可视化效果、过滤音频等。

```js
const audioContext = new (window.AudioContext || window.webkitAudioContext)();
const audioElement = document.getElementById('myAudio');
const track = audioContext.createMediaElementSource(audioElement);
const analyser = audioContext.createAnalyser();

track.connect(analyser);
analyser.connect(audioContext.destination);

const dataArray = new Uint8Array(analyser.frequencyBinCount);
function visualize() {
  analyser.getByteFrequencyData(dataArray);
  // 使用 dataArray 创建可视化效果
  requestAnimationFrame(visualize);
}
visualize();
```

### 综合示例

以下是一个综合示例，展示了如何使用 JavaScript 创建一个带有播放、暂停、音量控制、进度条和可视化效果的音频播放器。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced Audio Player</title>
  <style>
    .controls {
      display: flex;
      align-items: center;
    }
    button {
      margin-right: 10px;
    }
    input[type="range"] {
      margin-right: 10px;
    }
    canvas {
      border: 1px solid #000;
    }
  </style>
</head>
<body>
  <h1>Advanced Audio Player</h1>
  <audio id="myAudio">
    <source src="path/to/audio.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>
  <div class="controls">
    <button id="playButton">Play</button>
    <button id="pauseButton">Pause</button>
    <input type="range" id="volumeSlider" min="0" max="1" step="0.01" value="0.5">
    <input type="range" id="progressSlider" min="0" max="100" step="1" value="0">
  </div>
  <canvas id="visualizer" width="600" height="200"></canvas>
  <script src="script.js"></script>
</body>
</html>
```

```js
const audio = document.getElementById('myAudio');
const playButton = document.getElementById('playButton');
const pauseButton = document.getElementById('pauseButton');
const volumeSlider = document.getElementById('volumeSlider');
const progressSlider = document.getElementById('progressSlider');
const canvas = document.getElementById('visualizer');
const ctx = canvas.getContext('2d');

// 播放音频
playButton.addEventListener('click', () => {
  audio.play();
});

// 暂停音频
pauseButton.addEventListener('click', () => {
  audio.pause();
});

// 调整音量
volumeSlider.addEventListener('input', (e) => {
  audio.volume = e.target.value;
});

// 更新进度条
audio.addEventListener('timeupdate', () => {
  const progress = (audio.currentTime / audio.duration) * 100;
  progressSlider.value = progress;
});

// 跳转到指定时间
progressSlider.addEventListener('input', (e) => {
  const seekTime = (e.target.value / 100) * audio.duration;
  audio.currentTime = seekTime;
});

// 创建音频上下文和分析器
const audioContext = new (window.AudioContext || window.webkitAudioContext)();
const track = audioContext.createMediaElementSource(audio);
const analyser = audioContext.createAnalyser();

track.connect(analyser);
analyser.connect(audioContext.destination);

const dataArray = new Uint8Array(analyser.frequencyBinCount);

function visualize() {
  analyser.getByteFrequencyData(dataArray);
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  const barWidth = (canvas.width / dataArray.length) * 2.5;
  let barHeight;
  let x = 0;

  for (let i = 0; i < dataArray.length; i++) {
    barHeight = dataArray[i];
    ctx.fillStyle = 'rgb(' + (barHeight + 100) + ',50,50)';
    ctx.fillRect(x, canvas.height - barHeight / 2, barWidth, barHeight / 2);
    x += barWidth + 1;
  }

  requestAnimationFrame(visualize);
}

visualize();
```

**代码说明**

**HTML 部分：**

- 创建了一个 `<audio>` 元素，并提供音频文件的路径。
- 添加了播放、暂停按钮，音量控制滑块和进度条滑块。
- 添加了一个 `<canvas>` 元素用于可视化音频频谱。

**JavaScript 部分：**

- 获取 HTML 元素并添加事件监听器。
- 实现播放和暂停功能。
- 实现音量控制和进度条功能。
- 使用 Web Audio API 创建音频上下文和分析器。
- 实现音频频谱可视化效果。

## video

Video API 允许你在网页上播放、控制和操作视频文件。通过结合 HTML5 的 `<video>` 元素和 JavaScript，你可以创建功能丰富的视频播放器，控制视频的播放、暂停、音量、进度等。

### 基本使用

首先，在 HTML 中添加一个 `<video>` 元素，并提供视频文件的路径。

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Video Example</title>
</head>
<body>
  <h1>Video Example</h1>
  <video id="myVideo" width="640" height="360" controls>
    <source src="path/to/video.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <script src="script.js"></script>
</body>
</html>
```

你可以通过 JavaScript 获取 `<video>` 元素，并使用其方法和属性来控制视频播放。

```js
const video = document.getElementById('myVideo');

// 播放视频
video.play();

// 暂停视频
video.pause();

// 设置音量（范围 0.0 到 1.0）
video.volume = 0.5;

// 跳转到指定时间（秒）
video.currentTime = 30;
```

**视频控制示例**

以下是一个示例，展示了如何使用 JavaScript 创建一个简单的视频播放器，包含播放、暂停、音量控制和进度条。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Video Player</title>
  <style>
    .controls {
      display: flex;
      align-items: center;
      margin-top: 10px;
    }
    button {
      margin-right: 10px;
    }
    input[type="range"] {
      margin-right: 10px;
    }
  </style>
</head>
<body>
  <h1>Video Player</h1>
  <video id="myVideo" width="640" height="360">
    <source src="path/to/video.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <div class="controls">
    <button id="playButton">Play</button>
    <button id="pauseButton">Pause</button>
    <input type="range" id="volumeSlider" min="0" max="1" step="0.01" value="0.5">
    <input type="range" id="progressSlider" min="0" max="100" step="1" value="0">
  </div>
  <script src="script.js"></script>
</body>
</html>
```

```js
const video = document.getElementById('myVideo');
const playButton = document.getElementById('playButton');
const pauseButton = document.getElementById('pauseButton');
const volumeSlider = document.getElementById('volumeSlider');
const progressSlider = document.getElementById('progressSlider');

// 播放视频
playButton.addEventListener('click', () => {
  video.play();
});

// 暂停视频
pauseButton.addEventListener('click', () => {
  video.pause();
});

// 调整音量
volumeSlider.addEventListener('input', (e) => {
  video.volume = e.target.value;
});

// 更新进度条
video.addEventListener('timeupdate', () => {
  const progress = (video.currentTime / video.duration) * 100;
  progressSlider.value = progress;
});

// 跳转到指定时间
progressSlider.addEventListener('input', (e) => {
  const seekTime = (e.target.value / 100) * video.duration;
  video.currentTime = seekTime;
});
```

### 高级功能

#### 1. 处理视频事件

你可以监听视频元素的各种事件，如播放、暂停、结束、时间更新等，以便在这些事件发生时执行特定的操作。

```js
video.addEventListener('play', () => {
  console.log('Video is playing');
});

video.addEventListener('pause', () => {
  console.log('Video is paused');
});

video.addEventListener('ended', () => {
  console.log('Video has ended');
});

video.addEventListener('timeupdate', () => {
  console.log(`Current time: ${video.currentTime}`);
});
```

#### 2. 使用 Video 对象

除了使用 `<video>` 元素，你还可以直接使用 JavaScript 的 Video 对象来创建和控制视频。

```js
const video = document.createElement('video');
video.src = 'path/to/video.mp4';

// 播放视频
video.play();

// 暂停视频
video.pause();
```

#### 3. 视频分析和处理

使用 Canvas 和 WebRTC API，你可以进行更高级的视频处理和分析，例如创建视频特效、视频聊天等。

```js
const canvas = document.createElement('canvas');
const ctx = canvas.getContext('2d');

video.addEventListener('play', () => {
  const drawFrame = () => {
    if (!video.paused && !video.ended) {
      ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
      requestAnimationFrame(drawFrame);
    }
  };
  drawFrame();
});
```

#### 综合示例

以下是一个综合示例，展示了如何使用 JavaScript 创建一个带有播放、暂停、音量控制、进度条和视频特效的播放器。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Advanced Video Player</title>
  <style>
    .controls {
      display: flex;
      align-items: center;
      margin-top: 10px;
    }
    button {
      margin-right: 10px;
    }
    input[type="range"] {
      margin-right: 10px;
    }
    canvas {
      border: 1px solid #000;
      display: block;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <h1>Advanced Video Player</h1>
  <video id="myVideo" width="640" height="360">
    <source src="path/to/video.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
  <canvas id="videoCanvas" width="640" height="360"></canvas>
  <div class="controls">
    <button id="playButton">Play</button>
    <button id="pauseButton">Pause</button>
    <input type="range" id="volumeSlider" min="0" max="1" step="0.01" value="0.5">
    <input type="range" id="progressSlider" min="0" max="100" step="1" value="0">
  </div>
  <script src="script.js"></script>
</body>
</html>
```

```js
const video = document.getElementById('myVideo');
const canvas = document.getElementById('videoCanvas');
const ctx = canvas.getContext('2d');

const playButton = document.getElementById('playButton');
const pauseButton = document.getElementById('pauseButton');
const volumeSlider = document.getElementById('volumeSlider');
const progressSlider = document.getElementById('progressSlider');

// 播放视频
playButton.addEventListener('click', () => {
  video.play();
  drawFrame();
});

// 暂停视频
pauseButton.addEventListener('click', () => {
  video.pause();
});

// 调整音量
volumeSlider.addEventListener('input', (e) => {
  video.volume = e.target.value;
});

// 更新进度条
video.addEventListener('timeupdate', () => {
  const progress = (video.currentTime / video.duration) * 100;
  progressSlider.value = progress;
});

// 跳转到指定时间
progressSlider.addEventListener('input', (e) => {
  const seekTime = (e.target.value / 100) * video.duration;
  video.currentTime = seekTime;
});

// 视频特效
function drawFrame() {
  if (!video.paused && !video.ended) {
    ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
    applyGrayscaleEffect();
    requestAnimationFrame(drawFrame);
  }
}

function applyGrayscaleEffect() {
  const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
  const data = imageData.data;
  for (let i = 0; i < data.length; i += 4) {
    const avg = (data[i] + data[i + 1] + data[i + 2]) / 3;
    data[i] = avg; // 红色
    data[i + 1] = avg; // 绿色
    data[i + 2] = avg; // 蓝色
  }
  ctx.putImageData(imageData, 0, 0);
}
```

**代码说明**

**HTML 部分：**

- 创建了一个 `<video>` 元素，并提供视频文件的路径。将其设置为隐藏，因为我们会在 `<canvas>` 上显示视频。
- 添加了播放、暂停按钮，音量控制滑块和进度条滑块。
- 添加了一个 `<canvas>` 元素用于显示视频和特效。

**JavaScript 部分：**

- 获取 HTML 元素并添加事件监听器。
- 实现播放和暂停功能。
- 实现音量控制和进度条功能。
- 使用 Canvas API 实现视频特效（灰度处理）。
- 在视频播放时，使用 requestAnimationFrame 循环更新 Canvas 上的视频帧并应用特效。

## canvas

JavaScript 的 Canvas API 是 HTML5 提供的一个强大的绘图工具，使得在网页上绘制图形变得非常简单和灵活。Canvas 元素提供了一个可编程的、基于像素的绘图界面，可以用于绘制图形、制作动画、处理图像等。

### 基本使用

首先，你需要在 HTML 中添加一个 `<canvas>` 元素，然后通过 JavaScript 获取其上下文进行绘制。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Canvas Example</title>
  <style>
    canvas {
      border: 1px solid black;
    }
  </style>
</head>
<body>
  <h1>Canvas Example</h1>
  <canvas id="myCanvas" width="500" height="400"></canvas>
  <script src="script.js"></script>
</body>
</html>
```

```js
// 获取 Canvas 元素和上下文
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
```

### 绘制基本图形

**绘制矩形**

- fillRect(x, y, width, height): 绘制填充的矩形
- strokeRect(x, y, width, height): 绘制矩形的边框
- clearRect(x, y, width, height): 清除指定区域

```js
// 绘制填充的矩形
ctx.fillStyle = 'blue';
ctx.fillRect(50, 50, 150, 100);

// 绘制矩形的边框
ctx.strokeStyle = 'red';
ctx.strokeRect(250, 50, 150, 100);

// 清除矩形区域
ctx.clearRect(75, 75, 50, 50);
```

**绘制路径和线条**

- beginPath(): 开始新的路径
- moveTo(x, y): 移动画笔到指定位置
- lineTo(x, y): 从当前位置画一条直线到指定位置
- stroke(): 绘制路径
- closePath(): 闭合路径

```js
ctx.beginPath();
ctx.moveTo(100, 300);
ctx.lineTo(200, 300);
ctx.lineTo(150, 350);
ctx.closePath();
ctx.strokeStyle = 'green';
ctx.lineWidth = 5;
ctx.stroke();
```

**绘制圆形和弧线**

- arc(x, y, radius, startAngle, endAngle, anticlockwise): 绘制圆弧

```js
ctx.beginPath();
ctx.arc(300, 200, 50, 0, Math.PI * 2, true); // 绘制一个完整的圆
ctx.fillStyle = 'red';
ctx.fill();
```

**绘制文本**

- fillText(text, x, y): 绘制填充的文本
- strokeText(text, x, y): 绘制文本的边框

```js
ctx.font = '24px Arial';
ctx.fillStyle = 'black';
ctx.fillText('Hello, Canvas!', 200, 50);

ctx.strokeStyle = 'blue';
ctx.strokeText('Hello, Canvas!', 200, 100);
```

### 高级功能

**使用图像**

你可以在 Canvas 上绘制图像，使用 drawImage 方法。

```js
const img = new Image();
img.src = 'path/to/image.jpg';
img.onload = () => {
  ctx.drawImage(img, 50, 50, 200, 150);
};
```

**创建渐变**

- createLinearGradient(x0, y0, x1, y1): 创建线性渐变
- createRadialGradient(x0, y0, r0, x1, y1, r1): 创建径向渐变
- addColorStop(offset, color): 添加颜色停止点

```js
// 创建线性渐变
const linearGradient = ctx.createLinearGradient(0, 0, 500, 0);
linearGradient.addColorStop(0, 'red');
linearGradient.addColorStop(1, 'blue');

// 使用渐变填充矩形
ctx.fillStyle = linearGradient;
ctx.fillRect(0, 0, 500, 50);

// 创建径向渐变
const radialGradient = ctx.createRadialGradient(250, 250, 50, 250, 250, 200);
radialGradient.addColorStop(0, 'yellow');
radialGradient.addColorStop(1, 'green');

// 使用渐变填充圆形
ctx.fillStyle = radialGradient;
ctx.beginPath();
ctx.arc(250, 250, 200, 0, Math.PI * 2, true);
ctx.fill();
```

**动画**

通过使用 requestAnimationFrame 方法，你可以创建平滑的动画。

```js
let x = 0;
function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = 'blue';
  ctx.fillRect(x, 100, 50, 50);
  x += 1;
  requestAnimationFrame(animate);
}
animate();
```

### 综合示例

以下是一个综合示例，展示了如何在 Canvas 上绘制各种图形、文本和图像，并创建动画。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Canvas Comprehensive Example</title>
  <style>
    canvas {
      border: 1px solid black;
    }
  </style>
</head>
<body>
  <h1>Canvas Comprehensive Example</h1>
  <canvas id="myCanvas" width="500" height="400"></canvas>
  <script src="script.js"></script>
</body>
</html>
```

```js
// 获取 Canvas 元素和上下文
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');

// 绘制填充的矩形
ctx.fillStyle = 'blue';
ctx.fillRect(50, 50, 150, 100);

// 绘制矩形的边框
ctx.strokeStyle = 'red';
ctx.strokeRect(250, 50, 150, 100);

// 清除矩形区域
ctx.clearRect(75, 75, 50, 50);

// 绘制路径和线条
ctx.beginPath();
ctx.moveTo(100, 300);
ctx.lineTo(200, 300);
ctx.lineTo(150, 350);
ctx.closePath();
ctx.strokeStyle = 'green';
ctx.lineWidth = 5;
ctx.stroke();

// 绘制圆形
ctx.beginPath();
ctx.arc(300, 200, 50, 0, Math.PI * 2, true);
ctx.fillStyle = 'red';
ctx.fill();

// 绘制文本
ctx.font = '24px Arial';
ctx.fillStyle = 'black';
ctx.fillText('Hello, Canvas!', 200, 50);
ctx.strokeStyle = 'blue';
ctx.strokeText('Hello, Canvas!', 200, 100);

// 使用图像
const img = new Image();
img.src = 'path/to/image.jpg';
img.onload = () => {
  ctx.drawImage(img, 50, 50, 200, 150);
};

// 创建渐变
const linearGradient = ctx.createLinearGradient(0, 0, 500, 0);
linearGradient.addColorStop(0, 'red');
linearGradient.addColorStop(1, 'blue');
ctx.fillStyle = linearGradient;
ctx.fillRect(0, 0, 500, 50);

const radialGradient = ctx.createRadialGradient(250, 250, 50, 250, 250, 200);
radialGradient.addColorStop(0, 'yellow');
radialGradient.addColorStop(1, 'green');
ctx.fillStyle = radialGradient;
ctx.beginPath();
ctx.arc(250, 250, 200, 0, Math.PI * 2, true);
ctx.fill();

// 动画
let x = 0;
function animate() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = 'blue';
  ctx.fillRect(x, 100, 50, 50);
  x += 1;
  requestAnimationFrame(animate);
}
animate();
```






