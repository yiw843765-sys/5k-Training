<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Daily Workout & Progress</title>
  <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600&family=Quicksand:wght@500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-color: #FFF5F7;
      --card-bg: #FFFFFF;
      --primary-pink: #FF85A1;
      --soft-pink: #FFC2D1;
      --accent-purple: #B5179E;
      --text-dark: #4A4E69;
      --text-soft: #9A8C98;
      --mint: #70E4C8;
    }

    body {
      font-family: 'Quicksand', sans-serif;
      background-color: var(--bg-color);
      color: var(--text-dark);
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
    }

    .container {
      width: 100%;
      max-width: 450px;
      background: var(--card-bg);
      border-radius: 30px;
      padding: 25px;
      box-shadow: 0 10px 30px rgba(255, 133, 161, 0.15);
      border: 3px solid var(--soft-pink);
    }

    header {
      text-align: center;
      margin-bottom: 15px;
    }

    h1 {
      font-family: 'Fredoka', sans-serif;
      color: var(--primary-pink);
      font-size: 26px;
      margin: 5px 0;
    }

    .date-badge {
      display: inline-block;
      background: var(--soft-pink);
      color: var(--card-bg);
      font-weight: 700;
      padding: 6px 16px;
      border-radius: 20px;
      font-size: 14px;
      margin-top: 5px;
    }

    .workout-card {
      background: #FFF0F3;
      border-radius: 20px;
      padding: 18px;
      border: 2px dashed var(--primary-pink);
      margin-top: 15px;
    }

    .workout-title {
      font-family: 'Fredoka', sans-serif;
      color: var(--accent-purple);
      font-size: 19px;
      margin-bottom: 15px;
    }

    .exercise-block {
      background: var(--card-bg);
      padding: 12px 15px;
      border-radius: 15px;
      margin-bottom: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.03);
    }

    .exercise-name {
      font-size: 15px;
      font-weight: 700;
      margin-bottom: 8px;
    }

    .sets-row {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    .set-pill {
      background: #F0F0F0;
      color: var(--text-dark);
      padding: 6px 12px;
      border-radius: 12px;
      font-size: 12px;
      font-weight: 700;
      cursor: pointer;
      user-select: none;
      transition: all 0.2s ease;
      border: 1px solid #E0E0E0;
    }

    .set-pill.completed {
      background: var(--primary-pink);
      color: white;
      border-color: var(--primary-pink);
      text-decoration: line-through;
    }

    .upload-box {
      margin-top: 15px;
      background: #FDF2F8;
      border: 2px dotted var(--primary-pink);
      border-radius: 15px;
      padding: 12px;
      text-align: center;
    }

    .upload-box label {
      font-size: 13px;
      font-weight: 700;
      color: var(--accent-purple);
      cursor: pointer;
      display: block;
    }

    .upload-box input {
      margin-top: 8px;
      font-size: 12px;
    }

    .music-box {
      background: #F4EEFF;
      border-radius: 15px;
      padding: 12px 15px;
      margin-top: 20px;
      text-align: center;
      border: 1px solid #D8B4FE;
    }

    .music-title {
      font-weight: 700;
      font-size: 13px;
      color: var(--accent-purple);
      margin-bottom: 4px;
    }

    .song-name {
      font-size: 14px;
      font-weight: 700;
      color: var(--text-dark);
    }
  </style>
</head>
<body>

<div class="container">
  <header>
    <h1>✨ Daily Workout & Progress ✨</h1>
    <div class="date-badge" id="current-day-label">Loading...</div>
  </header>

  <div class="workout-card">
    <div class="workout-title" id="workout-title">🎯 Today's Mission</div>
    <div id="taskList"></div>

    <!-- Screenshot Upload Slot -->
    <div class="upload-box" id="upload-section">
      <label for="run-ss">📸 Upload Run Screenshot</label>
      <input type="file" id="run-ss" accept="image/*">
    </div>
  </div>

  <!-- Random Motivational Song Box -->
  <div class="music-box">
    <div class="music-title">🎵 Today's Hype Song</div>
    <div class="song-name" id="song-display">Loading track...</div>
  </div>
</div>

<script>
  const songs = [
    "🔥 'Eye of the Tiger' - Survivor",
    "⚡ 'Remember the Name' - Fort Minor",
    "👑 'Run the World (Girls)' - Beyoncé",
    "🚀 'Can't Hold Us' - Macklemore",
    "💪 'Stronger' - Kanye West",
    "💥 'Till I Collapse' - Eminem",
    "✨ 'Unstoppable' - Sia"
  ];

  const schedule = {
    0: {
      title: "🛌 Sunday Rest & Recovery",
      isRunDay: false,
      exercises: [
        { name: "Light Stretching & Foam Rolling", sets: ["10-15 Mins"] },
        { name: "Hydrate & Electrolytes", sets: ["Done"] }
      ]
    },
    1: {
      title: "🏋️‍♀️ Monday: Heavy Upper Push & Strength",
      isRunDay: false,
      exercises: [
        { name: "Dumbbell RDLs (20 lbs)", sets: ["Set 1 (8 reps)", "Set 2 (8 reps)", "Set 3 (8 reps)"] },
        { name: "Smith Machine Incline Push-ups", sets: ["Set 1 (10 reps)", "Set 2 (10 reps)", "Set 3 (10 reps)"] },
        { name: "Cable Seated Rows (17.5 lbs)", sets: ["Set 1 (10 reps)", "Set 2 (10 reps)", "Set 3 (10 reps)"] },
        { name: "Forearm Planks", sets: ["Set 1 (60s)", "Set 2 (60s)", "Set 3 (60s)"] }
      ]
    },
    2: {
      title: "👟 Tuesday: 2-Mile Aerobic Jog",
      isRunDay: true,
      exercises: [
        { name: "Warm-up Walk & Stretches", sets: ["5 Mins"] },
        { name: "2.0-Mile Jog (~10:00/mi)", sets: ["Mile 1", "Mile 2"] },
        { name: "Cool-down Walk & Leg Stretches", sets: ["5 Mins"] }
      ]
    },
    3: {
      title: "🧘‍♀️ Wednesday: Active Recovery & Mobility",
      isRunDay: false,
      exercises: [
        { name: "Easy Walk or Light Spin", sets: ["20-30 Mins"] },
        { name: "Hamstring & Calf Stretches", sets: ["Set 1", "Set 2"] },
        { name: "Cat-Cow & Cobra Stretches", sets: ["Set 1", "Set 2"] }
      ]
    },
    4: {
      title: "🔥 Thursday: Tempo Run + Power & Grip",
      isRunDay: true,
      exercises: [
        { name: "2.0-Mile Tempo Run (~9:40-9:50/mi)", sets: ["Mile 1", "Mile 2"] },
        { name: "Heavy Farmer's Carries (20-25 lb DBs)", sets: ["Set 1 (40m)", "Set 2 (40m)", "Set 3 (40m)", "Set 4 (40m)"] },
        { name: "Cable Horizontal Pallof Press", sets: ["Set 1 (10 reps)", "Set 2 (10 reps)", "Set 3 (10 reps)"] },
        { name: "Dumbbell Overhead Press (10-12.5 lbs)", sets: ["Set 1 (8-10 reps)", "Set 2 (8-10 reps)", "Set 3 (8-10 reps)"] }
      ]
    },
    5: {
      title: "⚡ Friday: Bodyweight Circuit & Core",
      isRunDay: false,
      exercises: [
        { name: "Incline Push-ups", sets: ["Round 1", "Round 2", "Round 3"] },
        { name: "Dumbbell Goblet Squats", sets: ["Round 1", "Round 2", "Round 3"] },
        { name: "Inverted Rows / Cable Pulldowns", sets: ["Round 1", "Round 2", "Round 3"] },
        { name: "Walking Lunges", sets: ["Round 1", "Round 2", "Round 3"] },
        { name: "Forearm Plank Hold", sets: ["Round 1 (45s)", "Round 2 (45s)", "Round 3 (45s)"] }
      ]
    },
    6: {
      title: "🏃‍♀️ Saturday: Long Aerobic Endurance Run",
      isRunDay: true,
      exercises: [
        { name: "Warm-up Walk", sets: ["5 Mins"] },
        { name: "2.5-Mile Run (~10:00-10:15/mi)", sets: ["Mile 1", "Mile 2", "0.5 Mi Finish"] },
        { name: "Cool-down Walk & Stretch", sets: ["5 Mins"] }
      ]
    }
  };

  const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
  const today = new Date();
  const dayOfWeek = today.getDay();

  document.getElementById('current-day-label').innerText = `${days[dayOfWeek]} Focus`;

  // Random Hype Song
  const randomSong = songs[Math.floor(Math.random() * songs.length)];
  document.getElementById('song-display').innerText = randomSong;

  const todayData = schedule[dayOfWeek];
  document.getElementById('workout-title').innerText = todayData.title;

  // Toggle Run Upload Box
  if (!todayData.isRunDay) {
    document.getElementById('upload-section').style.display = 'none';
  }

  const taskListContainer = document.getElementById('taskList');
  taskListContainer.innerHTML = '';

  todayData.exercises.forEach((item) => {
    const block = document.createElement('div');
    block.className = 'exercise-block';

    const title = document.createElement('div');
    title.className = 'exercise-name';
    title.innerText = item.name;
    block.appendChild(title);

    const setsRow = document.createElement('div');
    setsRow.className = 'sets-row';

    item.sets.forEach((setText) => {
      const pill = document.createElement('div');
      pill.className = 'set-pill';
      pill.innerText = setText;
      pill.onclick = function() {
        this.classList.toggle('completed');
      };
      setsRow.appendChild(pill);
    });

    block.appendChild(setsRow);
    taskListContainer.appendChild(block);
  });
</script>

</body>
</html>
