# awais
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Player</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: url('IMG-20211102-WA0000.jpg') no-repeat center center fixed ;
            background-size:contain;
            height: 100vh;
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            background-image: ;
        }

        .container {
            text-align: center;
            background-color: rgba(255, 255, 255, 0.7);
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
            width: 80%;
            max-width: 600px;
        }

        h1 {
            margin-bottom: 20px;
            color: #333;
        }

        .music-list {
            text-align: left;
            max-height: 200px;
            overflow-y: auto;
            padding: 10px;
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 8px;
            margin-top: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .music-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 8px 0;
            border-bottom: 1px solid #ccc;
        }

        .music-item:last-child {
            border-bottom: none;
        }

        .music-info {
            flex: 1;
            text-align: left;
            margin-left: 10px;
            color: #333;
        }

        .music-controls {
            display: flex;
            align-items: center;
        }

        .music-controls button {
            padding: 8px 16px;
            margin: 0 5px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        .music-controls button:hover {
            background-color: #0056b3;
        }

        .volume-slider {
            width: 100px;
            margin-left: 10px;
        }

        .download-button {
            margin-top: 10px;
        }

        .download-button button {
            padding: 10px 20px;
            background-color: #28a745;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        .download-button button:hover {
            background-color: #218838;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Music Player</h1>
        <input type="file" id="musicFile" accept=".mp3,.ogg" multiple />
        <div class="music-list" id="musicList"></div>
        <div class="music-controls">
            <button onclick="playAll()">Play All</button>
            <button onclick="pauseAll()">Pause All</button>
            <button onclick="stopAll()">Stop All</button>
        </div>
        <div class="download-button">
            <button onclick="downloadSelected()">Download Selected</button>
        </div>
    </div>

    <script>
        let musicPlayers = [];
        let musicFileInput = document.getElementById('musicFile');
        let musicList = document.getElementById('musicList');

        musicFileInput.addEventListener('change', function(event) {
            let files = event.target.files;

            for (let i = 0; i < files.length; i++) {
                let file = files[i];
                let url = URL.createObjectURL(file);

                let audio = new Audio();
                audio.src = url;
                audio.preload = 'auto';
                musicPlayers.push(audio);

                let musicItem = document.createElement('div');
                musicItem.classList.add('music-item');

                let musicInfo = document.createElement('div');
                musicInfo.classList.add('music-info');
                musicInfo.textContent = file.name;

                let musicControls = document.createElement('div');
                musicControls.classList.add('music-controls');

                let playButton = document.createElement('button');
                playButton.textContent = 'Play';
                playButton.onclick = function() {
                    togglePlay(audio);
                };

                let pauseButton = document.createElement('button');
                pauseButton.textContent = 'Pause';
                pauseButton.onclick = function() {
                    audio.pause();
                };

                let stopButton = document.createElement('button');
                stopButton.textContent = 'Stop';
                stopButton.onclick = function() {
                    audio.pause();
                    audio.currentTime = 0;
                };

                let volumeSlider = document.createElement('input');
                volumeSlider.type = 'range';
                volumeSlider.min = 0;
                volumeSlider.max = 1;
                volumeSlider.step = 0.1;
                volumeSlider.value = audio.volume;
                volumeSlider.classList.add('volume-slider');
                volumeSlider.oninput = function() {
                    audio.volume = volumeSlider.value;
                };

                musicControls.appendChild(playButton);
                musicControls.appendChild(pauseButton);
                musicControls.appendChild(stopButton);
                musicControls.appendChild(volumeSlider);

                musicItem.appendChild(musicInfo);
                musicItem.appendChild(musicControls);

                musicList.appendChild(musicItem);
            }
        });

        function togglePlay(audio) {
            if (audio.paused) {
                audio.play();
            } else {
                audio.pause();
            }
        }

        function playAll() {
            musicPlayers.forEach(function(player) {
                if (player.paused) {
                    player.play();
                }
            });
        }

        function pauseAll() {
            musicPlayers.forEach(function(player) {
                if (!player.paused) {
                    player.pause();
                }
            });
        }

        function stopAll() {
            musicPlayers.forEach(function(player) {
                player.pause();
                player.currentTime = 0;
            });
        }

        function downloadSelected() {
            let selectedAudio = musicPlayers.filter(player => !player.paused);
            if (selectedAudio.length > 0) {
                let blob = new Blob([selectedAudio[0].src], { type: 'audio/mpeg' });
                let url = URL.createObjectURL(blob);
                let a = document.createElement('a');
                a.href = url;
                a.download = selectedAudio[0].src.split('/').pop();
                a.click();
            }
        }
    </script>
</body>
</html>
