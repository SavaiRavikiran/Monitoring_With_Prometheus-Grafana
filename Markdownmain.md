<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Expandable Boxes</title>
  <style>
    .box {
      width: 200px;
      height: 50px;
      margin: 10px;
      padding: 10px;
      color: white;
      cursor: pointer;
      text-align: center;
      transition: height 0.3s ease;
    }
    .box-content {
      display: none;
      margin-top: 10px;
      color: white;
    }
    .box:hover {
      opacity: 0.9;
    }
    .expanded {
      height: 150px;
    }
    .box1 { background-color: #ff6347; } /* Tomato */
    .box2 { background-color: #ffa500; } /* Orange */
    .box3 { background-color: #ffd700; } /* Gold */
    .box4 { background-color: #32cd32; } /* Lime Green */
    .box5 { background-color: #1e90ff; } /* Dodger Blue */
  </style>
</head>
<body>
  <div class="box box1" onclick="toggleBox(this)">Box 1
    <div class="box-content">Expanded content for box 1</div>
  </div>
  <div class="box box2" onclick="toggleBox(this)">Box 2
    <div class="box-content">Expanded content for box 2</div>
  </div>
  <div class="box box3" onclick="toggleBox(this)">Box 3
    <div class="box-content">Expanded content for box 3</div>
  </div>
  <div class="box box4" onclick="toggleBox(this)">Box 4
    <div class="box-content">Expanded content for box 4</div>
  </div>
  <div class="box box5" onclick="toggleBox(this)">Box 5
    <div class="box-content">Expanded content for box 5</div>
  </div>

  <script>
    function toggleBox(element) {
      const content = element.querySelector('.box-content');
      if (content.style.display === 'none' || content.style.display === '') {
        content.style.display = 'block';
        element.classList.add('expanded');
      } else {
        content.style.display = 'none';
        element.classList.remove('expanded');
      }
    }
  </script>
</body>
</html>
