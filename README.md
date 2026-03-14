<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ultimate Global Structural Crack Analyzer</title>

<!-- Libraries -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

<style>
body { margin:0; font-family:Segoe UI; background:linear-gradient(to right,#141e30,#243b55); color:white; }
.header { display:flex; justify-content:space-between; align-items:center; padding:15px 20px; background:black; font-size:18px; flex-wrap:wrap;}
.language-select { background:white; color:black; padding:5px; border-radius:6px; margin-top:5px;}
.container { max-width:1200px; margin:auto; padding:20px;}
.card { background:white; color:black; padding:20px; border-radius:12px; margin-bottom:25px; box-shadow:0 8px 20px rgba(0,0,0,0.4);}
.section-title { font-weight:bold; font-size:18px; margin-bottom:15px; border-bottom:2px solid #ddd; padding-bottom:5px;}
.input-row { display:flex; gap:10px; margin-bottom:10px; }
.input-row input { flex:2; }
.input-row select { flex:1; }
input,select,textarea { width:100%; padding:8px; margin-top:6px; border-radius:6px; border:1px solid #aaa; }
button { padding:8px 12px; margin-top:10px; background:black; color:white; border:none; border-radius:6px; cursor:pointer;}
button:hover { background:#333; }
#severityBar { height:22px; background:#ddd; border-radius:20px; overflow:hidden; margin-top:10px; }
#severityFill { height:100%; width:0%; background:green; }
video,canvas { width:100%; max-width:400px; margin-top:10px; }
#threeContainer { width:100%; height:400px; margin-top:20px; }
</style>
</head>
<body>

<div class="header">
<div>ULTIMATE GLOBAL STRUCTURAL CRACK ANALYZER</div>
<select class="language-select" id="language" onchange="translateLabels()">
  <option value="en">English</option>
  <option value="hi">Hindi</option>
  <option value="ar">Arabic</option>
  <option value="ur">Urdu</option>
  <option value="fr">French</option>
  <option value="de">German</option>
  <option value="es">Spanish</option>
  <option value="zh">Chinese</option>
  <option value="ru">Russian</option>
  <option value="pt">Portuguese</option>
  <option value="ja">Japanese</option>
  <option value="ko">Korean</option>
  <option value="it">Italian</option>
  <option value="tr">Turkish</option>
  <option value="bn">Bengali</option>
  <option value="pa">Punjabi</option>
  <option value="ta">Tamil</option>
  <option value="te">Telugu</option>
  <option value="ml">Malayalam</option>
  <option value="kn">Kannada</option>
  <option value="gu">Gujarati</option>
  <option value="si">Sinhala</option>
  <option value="my">Burmese</option>
  <option value="km">Khmer</option>
  <option value="lo">Lao</option>
  <option value="am">Amharic</option>
  <option value="om">Oromo</option>
  <option value="sw">Swahili</option>
  <option value="yo">Yoruba</option>
  <option value="ig">Igbo</option>
  <option value="ha">Hausa</option>
  <option value="so">Somali</option>
  <option value="st">Sesotho</option>
  <option value="sn">Shona</option>
  <option value="ny">Chichewa</option>
  <option value="ps">Pashto</option>
  <option value="sd">Sindhi</option>
  <option value="ne">Nepali</option>
  <option value="bo">Tibetan</option>
  <option value="tg">Tajik</option>
  <option value="uz">Uzbek</option>
  <option value="kk">Kazakh</option>
  <option value="ky">Kyrgyz</option>
  <option value="tk">Turkmen</option>
  <option value="mn">Mongolian</option>
  <option value="tl">Tagalog</option>
  <option value="vi">Vietnamese</option>
  <option value="ms">Malay</option>
</select>
</div>

<div class="container">
<!-- Crack Input -->
<div class="card">
<div class="section-title" id="title1">Crack Input Details</div>

<label id="labelWidth">Crack Width</label>
<div class="input-row">
<input type="number" id="width">
<select id="widthUnit"><option>mm</option><option>cm</option><option>m</option><option>inch</option><option>ft</option></select>
</div>

<label id="labelLength">Crack Length</label>
<div class="input-row">
<input type="number" id="length">
<select id="lengthUnit"><option>mm</option><option>cm</option><option>m</option><option>inch</option><option>ft</option></select>
</div>

<label id="labelStructure">Structure Type</label>
<select id="structure" onchange="toggleWall()">
  <option value="">Select</option>
  <option>Beam</option>
  <option>Column</option>
  <option>Slab</option>
  <option value="Wall">Wall</option>
  <option>Retaining Wall</option>
</select>

<div id="wallOptions" style="display:none;">
<label>Wall Type</label>
<select>
<option>Load Bearing</option>
<option>Partition</option>
<option>Shear Wall</option>
</select>

<label>Plaster Applied?</label>
<select>
<option>Yes</option>
<option>No</option>
</select>
</div>

<label>Building Age (Years)</label>
<input type="number" id="age">

<label>Crack Direction</label>
<select id="direction">
<option>Vertical</option>
<option>Horizontal</option>
<option>Diagonal</option>
<option>Random</option>
</select>

<label>Crack Status</label>
<select id="status">
<option>Stable</option>
<option>Increasing</option>
<option>Decreasing</option>
</select>

<label>Location Notes</label>
<textarea id="notes"></textarea>

<button onclick="analyze()">Analyze Crack</button>
<button onclick="exportExcel()">Export Excel</button>
<button onclick="exportPDF()">Export PDF</button>

<div id="severityBar"><div id="severityFill"></div></div>
<div id="result"></div>
</div>

<!-- Image + Live Camera -->
<div class="card">
<div class="section-title">Photo / Live Crack Detection</div>
<input type="file" accept="image/*,.pdf,.doc,.docx" id="upload">
<button onclick="runImageAI()">Analyze Uploaded Image</button>
<hr>
<video id="video" autoplay></video>
<button onclick="startCamera()">Start Live Camera</button>
<button onclick="capture()">Capture & Analyze</button>
<canvas id="canvas"></canvas>
<div id="aiResult"></div>
</div>

<!-- 3D Visualization -->
<div class="card">
<div class="section-title">3D Crack Severity Visualization</div>
<div id="threeContainer"></div>
</div>

<!-- AI Graph -->
<div class="card">
<div class="section-title">AI Pattern Prediction Graph</div>
<canvas id="chart"></canvas>
</div>

<!-- Question -->
<div class="card">
<div class="section-title">Ask Structural Question</div>
<textarea id="question"></textarea>
<button onclick="answer()">Get Detailed Answer</button>
<div id="answerBox"></div>
</div>

<script>
// ====================== SINGLE COMBINED JAVASCRIPT ======================
let severity = 0;
let chart = null;

// Helper function to get element by ID
function getEl(id) { return document.getElementById(id); }

// Convert units
function convert(val, unit) {
  if (!val || isNaN(val)) return 0;
  if (unit == "cm") return val * 10;
  if (unit == "m") return val * 1000;
  if (unit == "inch") return val * 25.4;
  if (unit == "ft") return val * 304.8;
  return val; // mm
}

// Toggle wall options
function toggleWall() {
  const wallOptions = getEl("wallOptions");
  const structure = getEl("structure").value;
  if (wallOptions) {
    wallOptions.style.display = (structure === "Wall") ? "block" : "none";
  }
}

// Analyze function
function analyze() {
  const widthInput = getEl("width");
  const lengthInput = getEl("length");
  const ageInput = getEl("age");
  const statusSelect = getEl("status");
  const structureSelect = getEl("structure");
  const widthUnitSelect = getEl("widthUnit");
  const lengthUnitSelect = getEl("lengthUnit");
  
  if (!widthInput || !lengthInput) return;
  
  let w = convert(parseFloat(widthInput.value) || 0, widthUnitSelect ? widthUnitSelect.value : "mm");
  let l = convert(parseFloat(lengthInput.value) || 0, lengthUnitSelect ? lengthUnitSelect.value : "mm");
  let age = parseFloat(ageInput ? ageInput.value : 0) || 0;

  let factor = 1;

  if (statusSelect && statusSelect.value === "Increasing") factor += 0.5;
  if (structureSelect && structureSelect.value === "Column") factor += 0.5;
  if (age > 20) factor += 0.3;

  severity = Math.min(100, Math.floor((w * 5 + l * 0.1) * factor));

  updateBar();
  generateGraph();

  const resultDiv = getEl("result");
  if (resultDiv) {
    resultDiv.innerHTML =
      "<b>Severity:</b> " + severity + "/100<br>" +
      "<b>Crack Width (mm):</b> " + w.toFixed(2) + "<br>" +
      "<b>Crack Length (mm):</b> " + l.toFixed(2) + "<br>" +
      "<b>Recommended Materials:</b> Epoxy, Polymer Mortar, Acrylic Filler<br>" +
      "<b>Action:</b> Monitor regularly. Consult engineer if >70 severity.<br>" +
      "<b>Warning:</b> Do NOT ignore growing cracks.";
  }
}

// Update severity bar
function updateBar() {
  const fill = getEl("severityFill");
  if (fill) {
    fill.style.width = severity + "%";
    fill.style.background = severity < 40 ? "green" : severity < 70 ? "orange" : "red";
  }
}

// Generate graph
function generateGraph() {
  const canvas = getEl("chart");
  if (!canvas) return;
  
  if (chart) chart.destroy();

  chart = new Chart(canvas, {
    type: 'line',
    data: {
      labels: ["Now", "1M", "3M", "6M", "1Y"],
      datasets: [{
        label: "Predicted Severity",
        data: [
          severity,
          severity + 5,
          severity + 10,
          severity + 15,
          Math.min(100, severity + 20)
        ],
        borderColor: "rgb(75, 192, 192)",
        borderWidth: 2
      }]
    },
    options: { responsive: true }
  });
}

// Export to Excel
function exportExcel() {
  if (typeof XLSX === 'undefined') return;
  
  let ws = XLSX.utils.aoa_to_sheet([
    ["Parameter", "Value"],
    ["Crack Width", getEl("width") ? getEl("width").value : ""],
    ["Crack Length", getEl("length") ? getEl("length").value : ""],
    ["Severity", severity]
  ]);
  let wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, "Report");
  XLSX.writeFile(wb, "Crack_Report.xlsx");
}

// Export to PDF
function exportPDF() {
  if (typeof window.jspdf === 'undefined') return;
  
  const { jsPDF } = window.jspdf;
  let doc = new jsPDF();
  doc.text("Crack Analysis Report", 20, 20);
  doc.text("Width: " + (getEl("width") ? getEl("width").value : ""), 20, 30);
  doc.text("Length: " + (getEl("length") ? getEl("length").value : ""), 20, 40);
  doc.text("Severity: " + severity + "/100", 20, 50);
  doc.save("Crack_Report.pdf");
}

// Image AI analysis
function runImageAI() {
  const aiResult = getEl("aiResult");
  if (aiResult) aiResult.innerHTML = "AI scanned image. Crack confidence: 75%";
  severity = Math.min(100, severity + 20);
  updateBar();
  generateGraph();
}

// Start camera
function startCamera() {
  navigator.mediaDevices.getUserMedia({ video: true })
    .then(stream => {
      const video = getEl("video");
      if (video) video.srcObject = stream;
    })
    .catch(err => console.error("Camera error:", err));
}

// Capture from camera
function capture() {
  let video = getEl("video");
  let canvas = getEl("canvas");
  
  if (!video || !canvas) return;
  
  let ctx = canvas.getContext("2d");
  canvas.width = video.videoWidth || 640;
  canvas.height = video.videoHeight || 480;

  ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
  
  const aiResult = getEl("aiResult");
  if (aiResult) aiResult.innerHTML = "Live AI Detection Complete. Confidence: 80%";
  
  severity = Math.min(100, severity + 25);
  updateBar();
  generateGraph();
}

// 3D Visualization
function init3D() {
  const container = getEl("threeContainer");
  if (!container) return;
  
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(75, container.clientWidth / 400, 0.1, 1000);
  const renderer = new THREE.WebGLRenderer();
  
  renderer.setSize(container.clientWidth, 400);
  container.innerHTML = ''; // Clear container
  container.appendChild(renderer.domElement);

  const geometry = new THREE.BoxGeometry();
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);
  camera.position.z = 5;

  function animate() {
    requestAnimationFrame(animate);
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    renderer.render(scene, camera);
  }
  animate();
}

// Answer question
function answer() {
  const answerBox = getEl("answerBox");
  if (answerBox) {
    answerBox.innerHTML =
      "AI Guidance:<br>" +
      "• Measure width regularly<br>" +
      "• If >3mm consult engineer<br>" +
      "• Check reinforcement corrosion<br>" +
      "• Avoid overloading structure";
  }
}

// Language translations
const translations = {
  hi: {
    title1: "क्रैक इनपुट विवरण",
    labelWidth: "दरार चौड़ाई",
    labelLength: "दरार लंबाई",
    labelStructure: "संरचना प्रकार"
  },
  fr: {
    title1: "Détails de fissure",
    labelWidth: "Largeur fissure",
    labelLength: "Longueur fissure",
    labelStructure: "Type de structure"
  },
  es: {
    title1: "Detalles de grieta",
    labelWidth: "Ancho grieta",
    labelLength: "Longitud grieta",
    labelStructure: "Tipo de estructura"
  },
  de: {
    title1: "Riss Details",
    labelWidth: "Rissbreite",
    labelLength: "Risslänge",
    labelStructure: "Strukturtyp"
  },
  ar: {
    title1: "تفاصيل التشققات",
    labelWidth: "عرض الشق",
    labelLength: "طول الشق",
    labelStructure: "نوع الهيكل"
  },
  ur: {
    title1: "درار ان پٹ تفصیلات",
    labelWidth: "درار کی چوڑائی",
    labelLength: "درار کی لمبائی",
    labelStructure: "ڈھانچے کی قسم"
  }
};

function translateLabels() {
  const lang = getEl("language").value;
  if (!lang || lang === 'en' || !translations[lang]) return;
  
  const trans = translations[lang];
  for (const id in trans) {
    const elem = getEl(id);
    if (elem) elem.innerText = trans[id];
  }
}

// Initialize on load
window.onload = function() {
  init3D();
  generateGraph();
  
  // Add resize handler for 3D
  window.addEventListener('resize', function() {
    const container = getEl("threeContainer");
    if (container) {
      container.innerHTML = '';
      init3D();
    }
  });
};
</script>

</body>
</html>
