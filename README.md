<!DOCTYPE html>
<html>
<head>
  <title>Take a Photo</title>
</head>
<body>
  <h1>Take a Photo</h1>
  <video id="video" autoplay></video>
  <button id="capture">Capture</button>
  <canvas id="canvas" style="display:none;"></canvas>
  <script>
    const video = document.getElementById('video');
    const canvas = document.getElementById('canvas');
    const captureButton = document.getElementById('capture');

    // Access the camera
    navigator.mediaDevices.getUserMedia({ video: true })
      .then((stream) => {
        video.srcObject = stream;
      })
      .catch((err) => {
        console.error("Error accessing the camera: ", err);
      });

    // Capture the photo
    captureButton.addEventListener('click', () => {
      const context = canvas.getContext('2d');
      canvas.width = video.videoWidth;
      canvas.height = video.videoHeight;
      context.drawImage(video, 0, 0, canvas.width, canvas.height);

      // Convert the photo to a data URL
      const photo = canvas.toDataURL('image/png');

      // Send the photo to the server
      fetch('/upload', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ photo }),
      })
      .then(response => response.json())
      .then(data => {
        alert('Photo sent successfully!');
      })
      .catch((error) => {
        console.error('Error:', error);
      });
    });
  </script>
</body>
</html>
