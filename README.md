<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dành tặng Quyên </title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin-top: 50px;
    }

    #circle-container {
      position: relative;
      width: 200px;
      height: 200px;
      background-color: #f0f0f0;
      border-radius: 50%;
      margin: 0 auto;
      overflow: hidden; /* Đảm bảo chỉ hiển thị bên trong hình tròn */
    }

    .dot {
      position: absolute;
      width: 20px;
      height: 20px;
      background-color: red;
      border-radius: 50%;
      display: none;
    }

    #create-dot-btn {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }

    #create-new-circle-btn {
      margin-top: 10px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }
  </style>
</head>
<body>

<div id="circle-container">
  <!-- Dấu chấm sẽ xuất hiện ở đây -->
</div>

<button id="create-dot-btn" onclick="createRandomDot()">Buồn thì nhấn vào đây</button>

<script>
  function createRandomDot() {
    var circleContainer = document.getElementById('circle-container');

    // Tạo một div mới cho dấu chấm
    var dot = document.createElement('div');
    dot.className = 'dot';

    // Tạo tọa độ ngẫu nhiên cho dấu chấm bên trong hình tròn
    var randomX = Math.floor(Math.random() * (circleContainer.offsetWidth - dot.offsetWidth));
    var randomY = Math.floor(Math.random() * (circleContainer.offsetHeight - dot.offsetHeight));

    // Đặt vị trí của dấu chấm
    dot.style.left = randomX + 'px';
    dot.style.top = randomY + 'px';

    // Thêm dấu chấm vào hình tròn
    circleContainer.appendChild(dot);

    // Hiển thị dấu chấm
    dot.style.display = 'block';

    // Lưu trạng thái của dấu chấm vào localStorage
    saveDotState();

    // Kiểm tra xem có đủ dấu chấm trong hình tròn hay không
    checkDotCount();
  }

  function createNewCircle() {
    var circleContainer = document.getElementById('circle-container');

    // Xóa tất cả dấu chấm trong hình tròn
    while (circleContainer.firstChild) {
      circleContainer.removeChild(circleContainer.firstChild);
    }

    // Lưu trạng thái mới của hình tròn vào localStorage
    saveDotState();
  }

  function checkDotCount() {
    var circleContainer = document.getElementById('circle-container');
    var dotCount = circleContainer.querySelectorAll('.dot').length;

    // Nếu số lượng dấu chấm vượt quá một giới hạn nào đó, tạo hình tròn mới
    if (dotCount >= 10) { // Thay đổi giới hạn tùy ý
      createNewCircle();
    }
  }

  function saveDotState() {
    var circleContainer = document.getElementById('circle-container');
    var dots = circleContainer.querySelectorAll('.dot');

    var dotStates = [];

    dots.forEach(function(dot) {
      var dotState = {
        left: dot.style.left,
        top: dot.style.top
      };
      dotStates.push(dotState);
    });

    // Lưu trạng thái của tất cả các dấu chấm vào localStorage
    localStorage.setItem('dotStates', JSON.stringify(dotStates));
  }

  function restoreDotState() {
    var circleContainer = document.getElementById('circle-container');
    var dotStates = localStorage.getItem('dotStates');

    if (dotStates) {
      dotStates = JSON.parse(dotStates);

      dotStates.forEach(function(dotState) {
        var dot = document.createElement('div');
        dot.className = 'dot';
        dot.style.left = dotState.left;
        dot.style.top = dotState.top;
        circleContainer.appendChild(dot);
        dot.style.display = 'block';
      });
    }
  }

  // Khôi phục trạng thái của dấu chấm khi trang được tải lại
  window.onload = restoreDotState;

  // Lưu trạng thái của dấu chấm khi thoát hoặc làm mới trang
  window.onbeforeunload = saveDotState;
</script>

</body>
</html>

