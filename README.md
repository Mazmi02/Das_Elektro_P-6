<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Elektronika Kuantum (Quantum Electronics)?", "id": "Studi Interaksi Cahaya, Materi Kuantum." },
  { "en": "Apa Aplikasi Utama Elektronika Kuantum?", "id": "Laser, Maser, Jam Atom." },
  { "en": "Apa Itu Maser (Maser)?", "id": "Microwave Amplification Stimulated Emission Radiation." },
  { "en": "Apa Kepanjangan Maser (Microwave Amplification Stimulated Emission)?", "id": "Penguatan Gelombang Mikro Emisi Terstimulasi." },
  { "en": "Apa Itu Jam Atom (Atomic Clock)?", "id": "Jam Sangat Akurat Frekuensi Atom." },
  { "en": "Atom Apa Digunakan Jam Atom Umum?", "id": "Cesium (Cesium)." },
  { "en": "Apa Itu Kriptografi Kuantum (Quantum Cryptography)?", "id": "Metode Enkripsi Berbasis Mekanika Kuantum." },
  { "en": "Apa Itu Distribusi Kunci Kuantum (QKD)?", "id": "Quantum Key Distribution (QKD)." },
  { "en": "Apa Kepanjangan QKD (Quantum Key Distribution)?", "id": "Distribusi Kunci Kuantum." },
  { "en": "Apa Prinsip Dasar QKD (Quantum Key Distribution)?", "id": "Pengukuran Merusak Keadaan Kuantum (Deteksi Penyadap)." },
  { "en": "Apa Itu Sensor Kuantum (Quantum Sensor)?", "id": "Sensor Sensitivitas Tinggi Prinsip Kuantum." },
  { "en": "Apa Itu Metrologi Kuantum (Quantum Metrology)?", "id": "Pengukuran Presisi Tinggi Efek Kuantum." },
  { "en": "Apa Itu Efek Aharonov-Bohm (Aharonov-Bohm Effect)?", "id": "Partikel Bermuatan Dipengaruhi Potensial EM." },
  { "en": "Apa Itu Efek Hall Kuantum (Quantum Hall Effect)?", "id": "Kuantisasi Konduktansi Hall Medan Magnet Kuat." },
  { "en": "Apa Itu Superkonduktor Tipe I (Type I Superconductor)?", "id": "Medan Magnet Ditolak Sempurna (Meissner)." },
  { "en": "Apa Itu Superkonduktor Tipe II (Type II Superconductor)?", "id": "Medan Magnet Menembus Bentuk Vorteks." },
  { "en": "Apa Itu Vorteks Fluks (Flux Vortex)?", "id": "Garis Fluks Magnet Terkuantisasi Superkonduktor Tipe II." },
  { "en": "Apa Itu Teori BCS (Bardeen Cooper Schrieffer)?", "id": "Teori Mikroskopik Superkonduktivitas Konvensional." },
  { "en": "Apa Kepanjangan BCS (Bardeen Cooper Schrieffer)?", "id": "Bardeen, Cooper, Dan Schrieffer." },
  { "en": "Apa Itu Pasangan Cooper (Cooper Pair)?", "id": "Pasangan Elektron Terikat Superkonduktor." },
  { "en": "Apa Itu Celah Energi (Energy Gap) Superkonduktor?", "id": "Energi Minimum Pecah Pasangan Cooper." },
  { "en": "Apa Itu Efek Josephson (Josephson Effect)?", "id": "Arus Superkonduktor Lewati Sambungan Isolator Tipis." },
  { "en": "Apa Itu Sambungan Josephson (Josephson Junction)?", "id": "Dua Superkonduktor Dipisah Isolator Tipis." },
  { "en": "Apa Itu SQUID (Superconducting Quantum Interference Device)?", "id": "Sensor Medan Magnet Sangat Sensitif (Josephson)." },
  { "en": "Apa Itu Superkonduktor Suhu Tinggi (High-Tc)?", "id": "Superkonduktor Berbasis Keramik Tembaga Oksida." },
  { "en": "Apa Itu Magnesium Diborida (MgB₂)?", "id": "Superkonduktor Logam Suhu Transisi Relatif Tinggi." },
  { "en": "Apa Itu Kabel Superkonduktor (Superconducting Cable)?", "id": "Kabel Transmisi Daya Rugi Sangat Rendah." },
  { "en": "Apa Itu Pembatas Arus Gangguan Superkonduktor (SFCL)?", "id": "Superconducting Fault Current Limiter (SFCL)." },
  { "en": "Apa Kepanjangan SFCL (Superconducting Fault Current Limiter)?", "id": "Pembatas Arus Gangguan Superkonduktor." },
  { "en": "Apa Fungsi SFCL (Superconducting Fault Current Limiter)?", "id": "Membatasi Arus Hubung Singkat Cepat." },
  { "en": "Apa Itu Motor/Generator Superkonduktor (Superconducting Motor/Generator)?", "id": "Mesin Listrik Efisiensi Sangat Tinggi." },
  { "en": "Apa Itu Penyimpanan Energi Magnet Superkonduktor (SMES)?", "id": "Superconducting Magnetic Energy Storage (SMES)." },
  { "en": "Apa Kepanjangan SMES (Superconducting Magnetic Energy Storage)?", "id": "Penyimpanan Energi Magnet Superkonduktor." },
  { "en": "Apa Itu Kriogenik (Cryogenics)?", "id": "Teknologi Produksi, Penggunaan Suhu Rendah." },
  { "en": "Apa Itu Helium Cair (Liquid Helium) (LHe)?", "id": "Pendingin Suhu Sangat Rendah (Sekitar 4K)." },
  { "en": "Apa Itu Nitrogen Cair (Liquid Nitrogen) (LN₂)?", "id": "Pendingin Suhu Rendah (Sekitar 77K)." },
  { "en": "Apa Itu Cryocooler (Cryocooler)?", "id": "Pendingin Mekanis Suhu Kriogenik." },
  { "en": "Apa Itu Dewar (Dewar Flask)?", "id": "Wadah Termos Vakum Simpan Cairan Kriogenik." },
  { "en": "Apa Itu Rekayasa Perangkat Lunak Tertanam (Embedded Software)?", "id": "Pengembangan Software Sistem Tertanam." },
  { "en": "Apa Tantangan Software Tertanam (Embedded Software)?", "id": "Sumber Daya Terbatas, Real-Time, Keandalan." },
  { "en": "Apa Itu Cross-Compilation (Kompilasi Silang)?", "id": "Kompilasi Kode Target Berbeda Host." },
  { "en": "Apa Itu Debugger (Debugger) JTAG/SWD?", "id": "Antarmuka Debugging Hardware Mikrokontroler." },
  { "en": "Apa Kepanjangan SWD (Serial Wire Debug)?", "id": "Debug Kawat Serial." },
  { "en": "Apa Itu Analisis Waktu Statis (Static Timing Analysis)?", "id": "Verifikasi Batasan Waktu Desain Digital." },
  { "en": "Apa Itu Verifikasi Formal (Formal Verification)?", "id": "Pembuktian Matematis Kebenaran Desain." },
  { "en": "Apa Itu Emulasi (Emulation) Perangkat Keras?", "id": "Simulasi Hardware Menggunakan Hardware Khusus." },
  { "en": "Apa Itu Simulasi Siklus Akurat (Cycle Accurate Simulation)?", "id": "Simulasi Software Tiru Siklus Clock Hardware." },
  { "en": "Apa Itu Co-simulation (Ko-simulasi)?", "id": "Simulasi Bersama Komponen Hardware, Software." },
  { "en": "Apa Itu Desain Berbasis Model (Model Based Design)?", "id": "Pengembangan Sistem Menggunakan Model Grafis." },
  { "en": "Alat Apa Untuk Desain Berbasis Model?", "id": "MATLAB/Simulink." },
  { "en": "Apa Itu HIL (Hardware-in-the-Loop) Simulation?", "id": "Simulasi Hardware Dalam Loop." },
  { "en": "Apa Kepanjangan HIL (Hardware-in-the-Loop)?", "id": "Perangkat Keras Dalam Loop." },
  { "en": "Apa Fungsi Simulasi HIL (Hardware-in-the-Loop)?", "id": "Menguji Kontroler Tertanam Secara Realistis." },
  { "en": "Apa Itu SIL (Software-in-the-Loop) Simulation?", "id": "Simulasi Perangkat Lunak Dalam Loop." },
  { "en": "Apa Kepanjangan SIL (Software-in-the-Loop)?", "id": "Perangkat Lunak Dalam Loop." },
  { "en": "Apa Itu MIL (Model-in-the-Loop) Simulation?", "id": "Simulasi Model Dalam Loop." },
  { "en": "Apa Kepanjangan MIL (Model-in-the-Loop)?", "id": "Model Dalam Loop." },
  { "en": "Apa Itu Rapid Control Prototyping (RCP)?", "id": "Prototipe Cepat Sistem Kontrol." },
  { "en": "Apa Kepanjangan RCP (Rapid Control Prototyping)?", "id": "Prototipe Kontrol Cepat." },
  { "en": "Apa Itu Autocoding (Pembuatan Kode Otomatis)?", "id": "Menghasilkan Kode Otomatis Dari Model." },
  { "en": "Apa Itu Sistem Operasi (Operating System) (OS)?", "id": "Perangkat Lunak Pengelola Sumber Daya Komputer." },
  { "en": "Apa Fungsi Utama Sistem Operasi?", "id": "Manajemen Memori, Proses, Perangkat I/O." },
  { "en": "Apa Itu Kernel (Kernel) Sistem Operasi?", "id": "Inti Sistem Operasi." },
  { "en": "Apa Itu Penjadwalan (Scheduling) Proses?", "id": "Algoritma Alokasi Waktu CPU Proses." },
  { "en": "Algoritma Penjadwalan (Scheduling) Umum?", "id": "FIFO, Round Robin, Prioritas." },
  { "en": "Apa Itu Manajemen Memori (Memory Management)?", "id": "Alokasi, Dealokasi Memori Proses." },
  { "en": "Apa Itu Memori Virtual (Virtual Memory)?", "id": "Penggunaan Disk Sebagai Ekstensi RAM." },
  { "en": "Apa Itu Sistem Berkas (File System)?", "id": "Organisasi Penyimpanan Data Disk." },
  { "en": "Apa Itu Antarmuka Pengguna (User Interface)?", "id": "Cara Pengguna Interaksi Komputer." },
  { "en": "Jenis Antarmuka Pengguna (User Interface)?", "id": "CLI (Command Line), GUI (Graphical)." },
  { "en": "Apa Itu Driver (Driver) Perangkat?", "id": "Software Komunikasi OS, Hardware." },
  { "en": "Apa Itu Sistem Terdistribusi (Distributed System)?", "id": "Komponen Terpisah Komunikasi Lewat Jaringan." },
  { "en": "Tantangan Sistem Terdistribusi (Distributed System)?", "id": "Konkurensi, Kegagalan Parsial, Konsistensi." },
  { "en": "Apa Itu Komputasi Paralel (Parallel Computing)?", "id": "Eksekusi Banyak Tugas Bersamaan." },
  { "en": "Apa Beda Komputasi Paralel, Terdistribusi?", "id": "Paralel (Memori Bersama), Terdistribusi (Jaringan)." },
  { "en": "Apa Itu Arsitektur SIMD (Single Instruction Multiple Data)?", "id": "Satu Instruksi Operasi Banyak Data." },
  { "en": "Apa Kepanjangan SIMD (Single Instruction Multiple Data)?", "id": "Instruksi Tunggal, Data Ganda." },
  { "en": "Apa Itu Arsitektur MIMD (Multiple Instruction Multiple Data)?", "id": "Banyak Instruksi Operasi Banyak Data." },
  { "en": "Apa Kepanjangan MIMD (Multiple Instruction Multiple Data)?", "id": "Instruksi Ganda, Data Ganda." },
  { "en": "Apa Itu GPU (Graphics Processing Unit) Computing?", "id": "Penggunaan GPU Komputasi Paralel Umum." },
  { "en": "Apa Keunggulan GPU (Graphics Processing Unit) Computing?", "id": "Throughput Sangat Tinggi Tugas Paralel." },
  { "en": "Apa Itu CUDA (Compute Unified Device Architecture)?", "id": "Platform Komputasi Paralel NVIDIA." },
  { "en": "Apa Kepanjangan CUDA (Compute Unified Device Architecture)?", "id": "Arsitektur Perangkat Terpadu Komputasi." },
  { "en": "Apa Itu OpenCL (Open Computing Language)?", "id": "Standar Terbuka Komputasi Paralel Heterogen." },
  { "en": "Apa Kepanjangan OpenCL (Open Computing Language)?", "id": "Bahasa Komputasi Terbuka." },
  { "en": "Apa Itu Komputasi Kinerja Tinggi (HPC)?", "id": "High Performance Computing (HPC)." },
  { "en": "Apa Kepanjangan HPC (High Performance Computing)?", "id": "Komputasi Kinerja Tinggi." },
  { "en": "Dimana HPC (High Performance Computing) Digunakan?", "id": "Simulasi Ilmiah, Analisis Data Besar." },
  { "en": "Apa Itu Superkomputer (Supercomputer)?", "id": "Komputer Tercepat Dunia Pada Masanya." },
  { "en": "Ukuran Kinerja Superkomputer (Supercomputer)?", "id": "FLOPS (Floating Point Operations Per Second)." },
  { "en": "Apa Itu Cluster (Klaster) Komputer?", "id": "Sekelompok Komputer Bekerja Bersama." },
  { "en": "Apa Itu Komputasi Grid (Grid Computing)?", "id": "Pemanfaatan Sumber Daya Komputer Terdistribusi." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Layanan Komputasi Sesuai Permintaan Internet." },
  { "en": "Apa Itu Virtualisasi (Virtualization)?", "id": "Membuat Versi Virtual Sumber Daya Fisik." },
  { "en": "Apa Itu Mesin Virtual (Virtual Machine) (VM)?", "id": "Emulasi Sistem Komputer Perangkat Lunak." },
  { "en": "Apa Itu Hypervisor (Hypervisor)?", "id": "Perangkat Lunak Pengelola Mesin Virtual." },
  { "en": "Apa Itu Kontainer (Container)?", "id": "Virtualisasi Tingkat OS (Ringan)." },
  { "en": "Apa Itu Docker (Docker)?", "id": "Platform Populer Kontainerisasi." },
  { "en": "Apa Itu Kubernetes (Kubernetes)?", "id": "Platform Orkestrasi Kontainer." },
  { "en": "Apa Itu Komputasi Serverless (Serverless Computing)?", "id": "Eksekusi Fungsi Tanpa Kelola Server." },
  { "en": "Apa Itu Function as a Service (FaaS)?", "id": "Model Eksekusi Komputasi Serverless." },
  { "en": "Apa Itu Big Data (Big Data)?", "id": "Volume Data Sangat Besar, Kompleks, Cepat." },
  { "en": "Apa Itu Tiga V (V) Big Data?", "id": "Volume, Velocity (Kecepatan), Variety (Variasi)." },
  { "en": "Apa Itu V Keempat Big Data?", "id": "Veracity (Kebenaran)." },
  { "en": "Apa Itu V Kelima Big Data?", "id": "Value (Nilai)." },
  { "en": "Apa Itu Hadoop (Hadoop)?", "id": "Kerangka Kerja Ekosistem Big Data." },
  { "en": "Apa Itu HDFS (Hadoop Distributed File System)?", "id": "Sistem Berkas Penyimpanan Data Hadoop." },
  { "en": "Apa Itu MapReduce (MapReduce)?", "id": "Model Pemrograman Pemrosesan Data Hadoop." },
  { "en": "Apa Itu Spark (Apache Spark)?", "id": "Mesin Pemrosesan Data Cepat In-Memory." },
  { "en": "Apa Itu Basis Data NoSQL (Not Only SQL)?", "id": "Basis Data Non-Relasional Skala Besar." },
  { "en": "Apa Itu Sistem Otomatisasi Gedung (BAS)?", "id": "Building Automation System (BAS)." },
  { "en": "Apa Kepanjangan BAS (Building Automation System)?", "id": "Sistem Otomatisasi Gedung." },
  { "en": "Apa Fungsi BAS (Building Automation System)?", "id": "Kontrol HVAC, Pencahayaan, Keamanan Gedung." },
  { "en": "Apa Kepanjangan HVAC (Heating Ventilation Air Conditioning)?", "id": "Pemanasan, Ventilasi, Pendingin Udara." },
  { "en": "Protokol Komunikasi BAS (Building Automation System) Umum?", "id": "BACnet, LonWorks, Modbus." },
  { "en": "Apa Kepanjangan BACnet (Building Automation Control Network)?", "id": "Jaringan Kontrol Otomatisasi Gedung." },
  { "en": "Apa Itu DDC (Direct Digital Control)?", "id": "Kontroler Digital Langsung Sistem HVAC." },
  { "en": "Apa Kepanjangan DDC (Direct Digital Control)?", "id": "Kontrol Digital Langsung." },
  { "en": "Apa Itu Sensor Okupansi (Occupancy Sensor)?", "id": "Mendeteksi Kehadiran Orang Ruangan." },
  { "en": "Jenis Sensor Okupansi (Occupancy Sensor)?", "id": "PIR (Passive Infrared), Ultrasonik, Dual-Tech." },
  { "en": "Apa Fungsi Sensor Okupansi (Occupancy Sensor)?", "id": "Kontrol Lampu, HVAC Otomatis." },
  { "en": "Apa Itu Sensor Karbon Dioksida (CO₂)?", "id": "Mengukur Kadar CO₂ Udara." },
  { "en": "Mengapa Ukur CO₂ (Karbon Dioksida) Penting HVAC?", "id": "Indikator Kualitas Udara, Kontrol Ventilasi." },
  { "en": "Apa Itu Aktuator Damper (Damper Actuator)?", "id": "Penggerak Katup Udara Ventilasi." },
  { "en": "Apa Itu Katup Kontrol Air (Water Control Valve)?", "id": "Mengatur Aliran Air Panas/Dingin HVAC." },
  { "en": "Apa Itu Variable Air Volume (VAV) System?", "id": "Sistem HVAC Aliran Udara Variabel." },
  { "en": "Apa Kepanjangan VAV (Variable Air Volume)?", "id": "Volume Udara Variabel." },
  { "en": "Apa Keuntungan Sistem VAV (Variable Air Volume)?", "id": "Lebih Hemat Energi Dibanding CAV." },
  { "en": "Apa Kepanjangan CAV (Constant Air Volume)?", "id": "Volume Udara Konstan." },
  { "en": "Apa Itu Chiller (Pendingin) HVAC?", "id": "Mesin Pendingin Air Skala Besar." },
  { "en": "Apa Itu Boiler (Ketel Uap) HVAC?", "id": "Mesin Pemanas Air Atau Uap." },
  { "en": "Apa Itu Menara Pendingin (Cooling Tower)?", "id": "Mendinginkan Air Kondensor Chiller." },
  { "en": "Apa Itu Air Handling Unit (AHU)?", "id": "Unit Pengolah Udara HVAC." },
  { "en": "Apa Kepanjangan AHU (Air Handling Unit)?", "id": "Unit Pengolah Udara." },
  { "en": "Komponen Apa Ada Di AHU (Air Handling Unit)?", "id": "Filter, Koil Pendingin/Pemanas, Kipas." },
  { "en": "Apa Itu Fan Coil Unit (FCU)?", "id": "Unit Kipas Koil Skala Kecil Lokal." },
  { "en": "Apa Kepanjangan FCU (Fan Coil Unit)?", "id": "Unit Kipas Koil." },
  { "en": "Apa Itu Termostat (Thermostat)?", "id": "Sensor, Kontroler Suhu Ruangan." },
  { "en": "Apa Itu Sistem Manajemen Energi (EMS)?", "id": "Energy Management System (EMS)." },
  { "en": "Apa Kepanjangan EMS (Energy Management System)?", "id": "Sistem Manajemen Energi." },
  { "en": "Apa Fungsi EMS (Energy Management System) Gedung?", "id": "Optimasi Penggunaan Energi Gedung." },
  { "en": "Apa Itu Commissioning (Commissioning) Sistem BAS?", "id": "Verifikasi Kinerja Sistem Sesuai Desain." },
  { "en": "Apa Itu Rekayasa Nilai (Value Engineering)?", "id": "Analisis Fungsi, Biaya Optimasi Desain." },
  { "en": "Apa Itu Analisis Biaya Siklus Hidup (LCCA)?", "id": "Life Cycle Cost Analysis (LCCA)." },
  { "en": "Apa Kepanjangan LCCA (Life Cycle Cost Analysis)?", "id": "Analisis Biaya Siklus Hidup." },
  { "en": "Apa Itu Model Informasi Bangunan (BIM)?", "id": "Building Information Modeling (BIM)." },
  { "en": "Apa Kepanjangan BIM (Building Information Modeling)?", "id": "Pemodelan Informasi Bangunan." },
  { "en": "Apa Fungsi BIM (Building Information Modeling)?", "id": "Representasi Digital 3D Proyek Konstruksi." },
  { "en": "Apa Itu Sertifikasi Bangunan Hijau (Green Building)?", "id": "Pengakuan Bangunan Ramah Lingkungan." },
  { "en": "Contoh Sertifikasi Bangunan Hijau (Green Building)?", "id": "LEED (Leadership in Energy Environmental Design), Green Mark." },
  { "en": "Apa Kepanjangan LEED (Leadership in Energy Environmental Design)?", "id": "Kepemimpinan Desain Energi Lingkungan." },
  { "en": "Apa Itu Efisiensi Energi (Energy Efficiency)?", "id": "Menggunakan Energi Lebih Sedikit Hasil Sama." },
  { "en": "Apa Itu Audit Energi (Energy Audit)?", "id": "Inspeksi Penggunaan Energi Identifikasi Penghematan." },
  { "en": "Apa Itu Beban Pendinginan (Cooling Load)?", "id": "Jumlah Panas Perlu Dihilangkan Ruangan." },
  { "en": "Apa Itu Beban Pemanasan (Heating Load)?", "id": "Jumlah Panas Perlu Ditambahkan Ruangan." },
  { "en": "Apa Faktor Mempengaruhi Beban HVAC?", "id": "Cuaca, Insulasi, Penghuni, Peralatan." },
  { "en": "Apa Itu Insulasi Termal (Thermal Insulation)?", "id": "Bahan Menghambat Perpindahan Panas." },
  { "en": "Apa Itu Nilai-R (R-Value) Insulasi?", "id": "Ukuran Resistansi Termal Insulasi." },
  { "en": "Nilai-R (R-Value) Lebih Tinggi Berarti Apa?", "id": "Insulasi Lebih Baik." },
  { "en": "Apa Itu Jembatan Termal (Thermal Bridge)?", "id": "Area Konduktivitas Termal Tinggi (Rugi Panas)." },
  { "en": "Apa Itu Kaca Low-E (Low Emissivity)?", "id": "Kaca Lapisan Kurangi Transfer Panas Radiasi." },
  { "en": "Apa Itu Ventilasi (Ventilation)?", "id": "Proses Penggantian Udara Dalam Ruangan." },
  { "en": "Mengapa Ventilasi (Ventilation) Penting?", "id": "Kualitas Udara Dalam Ruangan (IAQ)." },
  { "en": "Apa Kepanjangan IAQ (Indoor Air Quality)?", "id": "Kualitas Udara Dalam Ruangan." },
  { "en": "Apa Itu Pemulihan Panas Ventilasi (HRV)?", "id": "Heat Recovery Ventilation (HRV)." },
  { "en": "Apa Kepanjangan HRV (Heat Recovery Ventilation)?", "id": "Ventilasi Pemulihan Panas." },
  { "en": "Apa Fungsi HRV (Heat Recovery Ventilation)?", "id": "Memindahkan Panas Udara Keluar Ke Masuk." },
  { "en": "Apa Itu Pemulihan Energi Ventilasi (ERV)?", "id": "Energy Recovery Ventilation (ERV)." },
  { "en": "Apa Kepanjangan ERV (Energy Recovery Ventilation)?", "id": "Ventilasi Pemulihan Energi." },
  { "en": "Apa Beda HRV (Heat Recovery Ventilation) Dan ERV (Energy Recovery Ventilation)?", "id": "ERV (Energy Recovery Ventilation) Juga Pindahkan Kelembaban." },
  { "en": "Apa Itu Psikrometrik (Psychrometrics)?", "id": "Studi Sifat Termodinamika Udara Lembab." },
  { "en": "Apa Itu Diagram Psikrometrik (Psychrometric Chart)?", "id": "Grafik Hubungan Properti Udara Lembab." },
  { "en": "Apa Itu Suhu Bola Kering (Dry Bulb)?", "id": "Suhu Udara Diukur Termometer Biasa." },
  { "en": "Apa Itu Suhu Bola Basah (Wet Bulb)?", "id": "Suhu Udara Pendinginan Evaporatif." },
  { "en": "Apa Itu Titik Embun (Dew Point)?", "id": "Suhu Udara Jenuh Uap Air." },
  { "en": "Apa Itu Kelembaban Relatif (Relative Humidity) (RH)?", "id": "Rasio Kandungan Uap Air Aktual, Maksimum." },
  { "en": "Apa Itu Entalpi (Enthalpy) Udara?", "id": "Total Kandungan Energi Panas Udara." },
  { "en": "Apa Itu Volume Spesifik (Specific Volume) Udara?", "id": "Volume Per Satuan Massa Udara." },
  { "en": "Proses Apa Ditampilkan Diagram Psikrometrik?", "id": "Pemanasan, Pendinginan, Pelembaban, Pengeringan." },
  { "en": "Apa Itu Pemanasan Sensibel (Sensible Heating)?", "id": "Pemanasan Tanpa Ubah Kandungan Air." },
  { "en": "Apa Itu Pendinginan Sensibel (Sensible Cooling)?", "id": "Pendinginan Tanpa Ubah Kandungan Air." },
  { "en": "Apa Itu Pelembaban (Humidification)?", "id": "Penambahan Uap Air Ke Udara." },
  { "en": "Apa Itu Pengeringan (Dehumidification)?", "id": "Pengurangan Uap Air Dari Udara." },
  { "en": "Apa Itu Pendinginan Evaporatif (Evaporative Cooling)?", "id": "Pendinginan Udara Melalui Penguapan Air." },
  { "en": "Apa Itu Koil Pendingin (Cooling Coil) AHU?", "id": "Penukar Panas Mendinginkan, Keringkan Udara." },
  { "en": "Apa Itu Koil Pemanas (Heating Coil) AHU?", "id": "Penukar Panas Memanaskan Udara." },
  { "en": "Apa Itu Filter Udara (Air Filter) AHU?", "id": "Menyaring Partikel Debu Udara." },
  { "en": "Apa Itu Peringkat MERV (Minimum Efficiency Reporting Value) Filter?", "id": "Ukuran Efisiensi Penyaringan Filter Udara." },
  { "en": "Apa Kepanjangan MERV (Minimum Efficiency Reporting Value)?", "id": "Nilai Pelaporan Efisiensi Minimum." },
  { "en": "MERV (Minimum Efficiency Reporting Value) Lebih Tinggi Berarti Apa?", "id": "Penyaringan Partikel Lebih Kecil, Efisien." },
  { "en": "Apa Itu Filter HEPA (High-Efficiency Particulate Air)?", "id": "Filter Efisiensi Sangat Tinggi." },
  { "en": "Apa Itu Kipas (Fan) Sentrifugal?", "id": "Kipas Udara Bergerak Radial Keluar." },
  { "en": "Apa Itu Kipas (Fan) Aksial?", "id": "Kipas Udara Bergerak Sejajar Poros." },
  { "en": "Apa Itu Tekanan Statis (Static Pressure) Kipas?", "id": "Tekanan Udara Atasi Hambatan Saluran." },
  { "en": "Apa Itu Tekanan Dinamis (Dynamic Pressure) Kipas?", "id": "Tekanan Akibat Kecepatan Udara." },
  { "en": "Apa Itu Tekanan Total (Total Pressure) Kipas?", "id": "Jumlah Tekanan Statis, Dinamis." },
  { "en": "Apa Itu Hukum Kipas (Fan Laws)?", "id": "Hubungan Kecepatan, Aliran, Tekanan Kipas." },
  { "en": "Apa Itu Saluran Udara (Ductwork) HVAC?", "id": "Sistem Saluran Distribusi Udara." },
  { "en": "Bahan Apa Untuk Saluran Udara (Ductwork)?", "id": "Logam Galvanis, Aluminium, Fiberglass." },
  { "en": "Apa Itu Damper (Damper) Udara?", "id": "Katup Pengatur Aliran Udara Saluran." },
  { "en": "Apa Itu Diffuser (Diffuser) Udara?", "id": "Outlet Penyebar Udara Ke Ruangan." },
  { "en": "Apa Itu Grille (Grille) Udara?", "id": "Kisi Penutup Lubang Udara Kembali." },
  { "en": "Apa Itu Penyeimbangan Udara (Air Balancing)?", "id": "Menyesuaikan Aliran Udara Sesuai Desain." },
  { "en": "Apa Itu Refrigeran (Refrigerant)?", "id": "Fluida Kerja Siklus Pendinginan." },
  { "en": "Contoh Refrigeran (Refrigerant) Umum?", "id": "R-410A, R-134a." },
  { "en": "Apa Itu Siklus Pendinginan Kompresi Uap?", "id": "Siklus Termodinamika Dasar Pendinginan AC." },
  { "en": "Komponen Utama Siklus Pendinginan?", "id": "Kompresor, Kondensor, Katup Ekspansi, Evaporator." },
  { "en": "Apa Fungsi Kompresor (Compressor)?", "id": "Menaikkan Tekanan, Suhu Refrigeran Gas." },
  { "en": "Apa Fungsi Kondensor (Condenser)?", "id": "Membuang Panas, Ubah Refrigeran Cair." },
  { "en": "Apa Fungsi Katup Ekspansi (Expansion Valve)?", "id": "Menurunkan Tekanan, Suhu Refrigeran Cair." },
  { "en": "Apa Fungsi Evaporator (Evaporator)?", "id": "Menyerap Panas, Ubah Refrigeran Gas." },
  { "en": "Bagian Mana Unit AC Indoor?", "id": "Evaporator (Evaporator) Dan Kipas Indoor." },
  { "en": "Bagian Mana Unit AC Outdoor?", "id": "Kompresor, Kondensor, Kipas Outdoor." },
  { "en": "Apa Itu Efek Elektrokalorik (Electrocaloric Effect)?", "id": "Perubahan Suhu Bahan Medan Listrik." },
  { "en": "Apa Itu Elektrostriksi (Electrostriction)?", "id": "Perubahan Bentuk Bahan Medan Listrik." },
  { "en": "Apa Beda Elektrostriksi, Piezoelektrik?", "id": "Elektrostriksi (Kuadrat Medan), Piezo (Linear)." },
  { "en": "Apa Itu Elektrolit Padat (Solid Electrolyte)?", "id": "Bahan Padat Konduktif Ion." },
  { "en": "Dimana Elektrolit Padat (Solid Electrolyte) Digunakan?", "id": "Baterai Solid-State, Sel Bahan Bakar." },
  { "en": "Apa Itu Sensor Ion Selektif (ISE)?", "id": "Ion Selective Electrode (ISE)." },
  { "en": "Apa Kepanjangan ISE (Ion Selective Electrode)?", "id": "Elektroda Ion Selektif." },
  { "en": "Apa Fungsi ISE (Ion Selective Electrode)?", "id": "Mengukur Konsentrasi Ion Tertentu Larutan." },
  { "en": "Contoh Penggunaan ISE (Ion Selective Electrode)?", "id": "Pengukuran pH (Ion H+)." },
  { "en": "Apa Itu Potensiometri (Potentiometry)?", "id": "Metode Pengukuran Potensial Elektroda." },
  { "en": "Apa Itu Amperometri (Amperometry)?", "id": "Metode Pengukuran Arus Reaksi Elektrokimia." },
  { "en": "Apa Itu Voltametri Siklik (Cyclic Voltammetry)?", "id": "Teknik Analisis Elektrokimia (Sapuan Tegangan)." },
  { "en": "Apa Itu Spektroskopi Impedansi Elektrokimia (EIS)?", "id": "Electrochemical Impedance Spectroscopy (EIS)." },
  { "en": "Apa Kepanjangan EIS (Electrochemical Impedance Spectroscopy)?", "id": "Spektroskopi Impedansi Elektrokimia." },
  { "en": "Apa Fungsi EIS (Electrochemical Impedance Spectroscopy)?", "id": "Menganalisis Proses Antarmuka Elektroda." },
  { "en": "Apa Itu Sel Surya Tersensitisasi Pewarna (DSSC)?", "id": "Dye Sensitized Solar Cell (DSSC)." },
  { "en": "Apa Kepanjangan DSSC (Dye Sensitized Solar Cell)?", "id": "Sel Surya Tersensitisasi Pewarna." },
  { "en": "Apa Prinsip Kerja DSSC (Dye Sensitized Solar Cell)?", "id": "Pewarna Menyerap Cahaya, Injeksi Elektron." },
  { "en": "Apa Itu Sel Surya Perovskite (Perovskite Solar Cell)?", "id": "Sel Surya Material Struktur Perovskite." },
  { "en": "Apa Keunggulan Sel Surya Perovskite?", "id": "Efisiensi Tinggi Potensial, Biaya Rendah." },
  { "en": "Apa Itu Sel Surya Organik (Organic Solar Cell)?", "id": "Sel Surya Bahan Semikonduktor Organik." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot) Sel Surya?", "id": "Sel Surya Menggunakan Titik Kuantum." },
  { "en": "Apa Itu Efisiensi Konversi Daya (PCE)?", "id": "Power Conversion Efficiency (PCE) Sel Surya." },
  { "en": "Apa Kepanjangan PCE (Power Conversion Efficiency)?", "id": "Efisiensi Konversi Daya." },
  { "en": "Apa Itu Faktor Isi (Fill Factor) (FF) Sel Surya?", "id": "Ukuran Kualitas Kurva I-V Sel Surya." },
  { "en": "Apa Rumus Faktor Isi (Fill Factor) (FF)?", "id": "FF = (Imp * Vmp) / (Isc * Voc)." },
  { "en": "Apa Itu Radiasi Matahari (Solar Irradiance)?", "id": "Daya Radiasi Matahari Per Luas Area." },
  { "en": "Satuan Apa Radiasi Matahari (Solar Irradiance)?", "id": "Watt Per Meter Persegi (W/m²)." },
  { "en": "Apa Itu Kondisi Uji Standar (STC) Sel Surya?", "id": "Standard Test Conditions (STC)." },
  { "en": "Apa Kepanjangan STC (Standard Test Conditions)?", "id": "Kondisi Uji Standar." },
  { "en": "Apa Kondisi STC (Standard Test Conditions)?", "id": "Iradiasi 1000 W/m², Suhu 25°C, AM1.5." },
  { "en": "Apa Itu Massa Udara (Air Mass) (AM)?", "id": "Ukuran Jarak Tempuh Cahaya Atmosfer." },
  { "en": "Apa Itu Pelacak Titik Daya Maksimum (MPPT)?", "id": "Maximum Power Point Tracking (MPPT)." },
  { "en": "Apa Fungsi MPPT (Maximum Power Point Tracking)?", "id": "Mengoperasikan Panel Surya Daya Maksimal." },
  { "en": "Apa Itu Inverter Terhubung Jaringan (Grid-Tied Inverter)?", "id": "Inverter Sinkronisasi Jaringan Listrik PLN." },
  { "en": "Apa Itu Inverter Mandiri (Off-Grid Inverter)?", "id": "Inverter Untuk Sistem Tanpa Jaringan PLN." },
  { "en": "Apa Itu Inverter Hibrida (Hybrid Inverter)?", "id": "Inverter Bisa Mode Grid-Tied, Off-Grid." },
  { "en": "Apa Itu Kontroler Pengisian (Charge Controller) Surya?", "id": "Mengatur Pengisian Baterai Panel Surya." },
  { "en": "Jenis Kontroler Pengisian (Charge Controller) Surya?", "id": "PWM (Pulse Width Modulation) Dan MPPT (Maximum Power Point Tracking)." },
  { "en": "Mana Lebih Efisien, PWM (Pulse Width Modulation) Atau MPPT (Maximum Power Point Tracking)?", "id": "MPPT (Maximum Power Point Tracking) Umumnya Lebih Efisien." },
  { "en": "Apa Itu Elektroliser (Electrolyzer)?", "id": "Alat Pemisah Air (H₂O) Jadi H₂, O₂." },
  { "en": "Apa Itu Produksi Hidrogen (Hydrogen Production) Hijau?", "id": "Elektrolisis Menggunakan Energi Terbarukan." },
  { "en": "Apa Itu Ekonomi Hidrogen (Hydrogen Economy)?", "id": "Konsep Penggunaan Hidrogen Sumber Energi." },
  { "en": "Apa Itu Penyimpanan Energi Udara Terkompresi (CAES)?", "id": "Compressed Air Energy Storage (CAES)." },
  { "en": "Apa Kepanjangan CAES (Compressed Air Energy Storage)?", "id": "Penyimpanan Energi Udara Terkompresi." },
  { "en": "Apa Itu Penyimpanan Energi Roda Gila (Flywheel)?", "id": "Menyimpan Energi Kinetik Roda Berputar." },
  { "en": "Apa Itu Penyimpanan Energi Panas (Thermal Energy Storage)?", "id": "Menyimpan Energi Dalam Bentuk Panas." },
  { "en": "Contoh Penyimpanan Energi Panas (Thermal Energy Storage)?", "id": "Garam Cair (Molten Salt)." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya Terkonsentrasi (CSP)?", "id": "Concentrated Solar Power (CSP)." },
  { "en": "Apa Kepanjangan CSP (Concentrated Solar Power)?", "id": "Tenaga Surya Terkonsentrasi." },
  { "en": "Bagaimana CSP (Concentrated Solar Power) Bekerja?", "id": "Memfokuskan Cahaya Matahari Panaskan Fluida." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Ombak (Wave Power)?", "id": "Menghasilkan Listrik Energi Gelombang Laut." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Pasang Surut (Tidal Power)?", "id": "Menghasilkan Listrik Energi Pasang Surut." },
  { "en": "Apa Itu Konversi Energi Termal Lautan (OTEC)?", "id": "Ocean Thermal Energy Conversion (OTEC)." },
  { "en": "Apa Kepanjangan OTEC (Ocean Thermal Energy Conversion)?", "id": "Konversi Energi Termal Lautan." },
  { "en": "Prinsip Apa Digunakan OTEC (Ocean Thermal Energy Conversion)?", "id": "Perbedaan Suhu Air Laut Permukaan, Dalam." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Panas Bumi (Geothermal)?", "id": "Memanfaatkan Panas Dari Dalam Bumi." },
  { "en": "Apa Itu Sumur Produksi (Production Well) Geotermal?", "id": "Sumur Mengambil Uap/Air Panas Bumi." },
  { "en": "Apa Itu Sumur Injeksi (Injection Well) Geotermal?", "id": "Sumur Mengembalikan Air Ke Reservoir." },
  { "en": "Apa Itu Reservoir (Reservoir) Panas Bumi?", "id": "Lapisan Batuan Panas Mengandung Fluida." },
  { "en": "Apa Itu Pembangkit Flash Steam (Flash Steam) Geotermal?", "id": "Air Panas Di-flash Jadi Uap Putar Turbin." },
  { "en": "Apa Itu Pembangkit Dry Steam (Dry Steam) Geotermal?", "id": "Langsung Gunakan Uap Kering Dari Bumi." },
  { "en": "Apa Itu Pembangkit Siklus Biner (Binary Cycle) Geotermal?", "id": "Gunakan Fluida Kerja Sekunder Titik Didih Rendah." },
  { "en": "Apa Itu Transmisi Listrik (Electricity Transmission)?", "id": "Penyaluran Daya Besar Jarak Jauh." },
  { "en": "Apa Tegangan Transmisi (Transmission Voltage) Umum?", "id": "Tegangan Tinggi (HV), Ekstra Tinggi (EHV)." },
  { "en": "Apa Kepanjangan HV (High Voltage)?", "id": "Tegangan Tinggi." },
  { "en": "Apa Kepanjangan EHV (Extra High Voltage)?", "id": "Tegangan Ekstra Tinggi." },
  { "en": "Apa Kepanjangan UHV (Ultra High Voltage)?", "id": "Tegangan Ultra Tinggi." },
  { "en": "Apa Itu Saluran Udara Tegangan Tinggi (Overhead Line)?", "id": "Transmisi Menggunakan Menara, Konduktor Udara." },
  { "en": "Apa Konduktor Umum Saluran Udara?", "id": "ACSR (Aluminum Conductor Steel Reinforced)." },
  { "en": "Apa Fungsi Menara Transmisi (Transmission Tower)?", "id": "Menyangga Konduktor, Jaga Jarak Aman." },
  { "en": "Apa Fungsi Isolator (Insulator) Transmisi?", "id": "Mengisolasi Konduktor Dari Menara (Tanah)." },
  { "en": "Apa Jenis Isolator (Insulator) Transmisi?", "id": "Isolator Pin, Gantung (Suspension), Post." },
  { "en": "Apa Itu Kawat Tanah (Ground Wire) Saluran Udara?", "id": "Kawat Teratas Melindungi Dari Petir." },
  { "en": "Nama Lain Kawat Tanah (Ground Wire)?", "id": "Kawat Pelindung (Shield Wire)." },
  { "en": "Apa Itu Kabel Bawah Tanah (Underground Cable)?", "id": "Transmisi Menggunakan Kabel Ditanam." },
  { "en": "Keuntungan Kabel Bawah Tanah (Underground Cable)?", "id": "Estetika, Tahan Cuaca." },
  { "en": "Kerugian Kabel Bawah Tanah (Underground Cable)?", "id": "Biaya Mahal, Sulit Perbaikan, Kapasitansi Tinggi." },
  { "en": "Apa Itu Efek Kapasitansi (Capacitance Effect) Kabel Bawah Tanah?", "id": "Menghasilkan Arus Pengisian Kapasitif." },
  { "en": "Apa Itu Jaringan Distribusi (Distribution Network)?", "id": "Menyalurkan Listrik Gardu Induk Ke Konsumen." },
  { "en": "Apa Tegangan Distribusi (Distribution Voltage) Primer?", "id": "Tegangan Menengah (Contoh: 20 kV)." },
  { "en": "Apa Tegangan Distribusi (Distribution Voltage) Sekunder?", "id": "Tegangan Rendah (Contoh: 220/380 V)." },
  { "en": "Apa Itu Trafo Distribusi (Distribution Transformer)?", "id": "Menurunkan Tegangan Primer Ke Sekunder." },
  { "en": "Dimana Trafo Distribusi (Distribution Transformer) Dipasang?", "id": "Tiang Listrik Atau Gardu Beton." },
  { "en": "Apa Itu Jaringan Distribusi (Distribution Network) Radial?", "id": "Satu Arah Sumber Ke Beban." },
  { "en": "Apa Itu Jaringan Distribusi (Distribution Network) Loop?", "id": "Beban Dapat Disuplai Dua Arah." },
  { "en": "Apa Keuntungan Jaringan Loop (Loop Network)?", "id": "Keandalan Lebih Tinggi." },
  { "en": "Apa Itu Recloser (Recloser) Distribusi?", "id": "Pemutus Otomatis Menutup Kembali (Gangguan Sementara)." },
  { "en": "Apa Itu Sectionalizer (Sectionalizer) Distribusi?", "id": "Pemisah Otomatis Isolasi Gangguan Permanen." },
  { "en": "Apa Itu Sekring Pelebur (Fuse Cutout) Distribusi?", "id": "Pelindung Arus Lebih Sisi Primer Trafo." },
  { "en": "Apa Itu Arester Surja (Surge Arrester) Distribusi?", "id": "Pelindung Peralatan Dari Surja Petir." },
  { "en": "Apa Itu Regulator Tegangan (Voltage Regulator) Distribusi?", "id": "Menjaga Tegangan Jaringan Tetap Stabil." },
  { "en": "Bagaimana Regulator Tegangan (Voltage Regulator) Bekerja?", "id": "Mengubah Tap Transformator Otomatis." },
  { "en": "Apa Itu Kapasitor Bank (Capacitor Bank) Distribusi?", "id": "Memperbaiki Faktor Daya, Menaikkan Tegangan." },
  { "en": "Apa Itu Otomatisasi Distribusi (Distribution Automation)?", "id": "Penggunaan Teknologi Kontrol, Monitor Jaringan." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Modernisasi Jaringan Listrik Komunikasi Digital." },
  { "en": "Apa Komponen Utama Smart Grid (Jaringan Cerdas)?", "id": "Meter Cerdas, Sensor Jaringan, Komunikasi, Analitik." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Elektronik Komunikasi Dua Arah." },
  { "en": "Apa Manfaat Meter Cerdas (Smart Meter)?", "id": "Pembacaan Otomatis, Manajemen Beban, Deteksi Gangguan." },
  { "en": "Apa Itu Advanced Metering Infrastructure (AMI)?", "id": "Infrastruktur Jaringan Komunikasi Meter Cerdas." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Mempengaruhi Pola Konsumsi Listrik Pelanggan." },
  { "en": "Apa Itu Respon Beban (Demand Response)?", "id": "Pelanggan Mengurangi Beban Saat Sinyal." },
  { "en": "Apa Itu Pembangkit Terdistribusi (Distributed Generation) (DG)?", "id": "Pembangkit Skala Kecil Terhubung Distribusi." },
  { "en": "Apa Itu Jaringan Mikro (Microgrid)?", "id": "Bagian Jaringan Bisa Operasi Mandiri." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Menyuplai Daya Jaringan." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Cabang Fisika Listrik Dan Magnet." },
  { "en": "Siapa Perumus Teori Elektromagnetik Klasik?", "id": "James Clerk Maxwell." },
  { "en": "Apa Itu Muatan Listrik (Electric Charge)?", "id": "Sifat Dasar Materi (Positif/Negatif)." },
  { "en": "Apa Satuan Muatan Listrik (Electric Charge)?", "id": "Coulomb (C)." },
  { "en": "Apa Hukum Coulomb (Coulomb's Law)?", "id": "Gaya Antara Dua Muatan Titik." },
  { "en": "Apa Itu Medan Listrik (Electric Field) (E)?", "id": "Gaya Listrik Per Satuan Muatan." },
  { "en": "Apa Satuan Medan Listrik (Electric Field)?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Itu Garis Medan Listrik (Electric Field Lines)?", "id": "Representasi Arah Medan Listrik." },
  { "en": "Arah Garis Medan Listrik (Electric Field Lines)?", "id": "Dari Muatan Positif Ke Negatif." },
  { "en": "Apa Itu Fluks Listrik (Electric Flux)?", "id": "Ukuran Jumlah Medan Listrik Menembus Luas." },
  { "en": "Apa Hukum Gauss (Gauss's Law) Listrik?", "id": "Fluks Listrik Total Proporsional Muatan Tertutup." },
  { "en": "Apa Itu Potensial Listrik (Electric Potential) (V)?", "id": "Energi Potensial Listrik Per Muatan." },
  { "en": "Apa Satuan Potensial Listrik (Electric Potential)?", "id": "Volt (V)." },
  { "en": "Apa Itu Tegangan (Voltage)?", "id": "Perbedaan Potensial Listrik Dua Titik." },
  { "en": "Apa Hubungan Medan Listrik (E), Potensial (V)?", "id": "E = -Gradien(V)." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Penyimpan Energi Medan Listrik." },
  { "en": "Apa Satuan Kapasitansi (Capacitance)?", "id": "Farad (F)." },
  { "en": "Apa Itu Dielektrik (Dielectric)?", "id": "Bahan Isolator Dalam Kapasitor." },
  { "en": "Apa Itu Arus (Current) Listrik (I)?", "id": "Laju Aliran Muatan Listrik." },
  { "en": "Apa Satuan Arus (Current)?", "id": "Ampere (A)." },
  { "en": "Apa Itu Resistansi (Resistance) (R)?", "id": "Hambatan Terhadap Aliran Arus." },
  { "en": "Apa Satuan Resistansi (Resistance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Hukum Ohm (Ohm's Law)?", "id": "V = I * R." },
  { "en": "Apa Itu Resistivitas (Resistivity) (ρ)?", "id": "Hambatan Jenis Intrinsik Material." },
  { "en": "Apa Itu Daya (Power) Listrik (P)?", "id": "Laju Transfer Energi Listrik (Watt)." },
  { "en": "Apa Rumus Daya (Power) (P)?", "id": "P = V * I = I²R = V²/R." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Jumlah Arus Masuk Simpul = Keluar." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Jumlah Tegangan Loop Tertutup = Nol." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field) (B)?", "id": "Medan Dihasilkan Magnet/Arus Bergerak." },
  { "en": "Apa Satuan Medan Magnet (Magnetic Field) (B)?", "id": "Tesla (T)." },
  { "en": "Apa Itu Garis Medan Magnet (Magnetic Field Lines)?", "id": "Representasi Arah Medan Magnet." },
  { "en": "Arah Garis Medan Magnet (Magnetic Field Lines)?", "id": "Dari Kutub Utara Ke Selatan." },
  { "en": "Apakah Monopol Magnetik (Magnetic Monopole) Ada?", "id": "Secara Teoretis Belum Terbukti Ada." },
  { "en": "Apa Hukum Gauss (Gauss's Law) Magnet?", "id": "Fluks Magnet Total Lewat Permukaan Tertutup Nol." },
  { "en": "Apa Hukum Biot-Savart (Biot-Savart Law)?", "id": "Menghitung Medan Magnet Akibat Elemen Arus." },
  { "en": "Apa Hukum Ampere (Ampere's Law)?", "id": "Hubungan Integral Medan Magnet Sekitar Arus." },
  { "en": "Apa Modifikasi Maxwell (Maxwell Modification) Hukum Ampere?", "id": "Menambahkan Suku Arus Perpindahan." },
  { "en": "Apa Itu Arus Perpindahan (Displacement Current)?", "id": "Arus Efektif Akibat Perubahan Medan Listrik." },
  { "en": "Apa Itu Gaya Lorentz (Lorentz Force)?", "id": "Gaya Total Medan E, B Muatan Bergerak." },
  { "en": "Apa Rumus Gaya Lorentz (Lorentz Force)?", "id": "F = q(E + v x B)." },
  { "en": "Apa Itu Gaya Magnetik (Magnetic Force) Kawat Berarus?", "id": "Gaya Pada Kawat Medan Magnet (F=ILB)." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Pembangkitan GGL Medan Magnet Berubah." },
  { "en": "Apa Hukum Faraday (Faraday's Law) Induksi?", "id": "GGL Induksi = - Laju Perubahan Fluks." },
  { "en": "Apa Tanda Negatif Hukum Faraday (Faraday's Law)?", "id": "Menunjukkan Arah Sesuai Hukum Lenz." },
  { "en": "Apa Itu Hukum Lenz (Lenz's Law)?", "id": "Arus Induksi Melawan Perubahan Penyebabnya." },
  { "en": "Apa Itu Induktansi (Inductance) (L)?", "id": "Ukuran Kemampuan Hasilkan GGL Induksi Diri." },
  { "en": "Apa Satuan Induktansi (Inductance)?", "id": "Henry (H)." },
  { "en": "Apa Itu Induktansi Mutual (Mutual Inductance) (M)?", "id": "Induksi GGL Satu Kumparan Akibat Lainnya." },
  { "en": "Apa Itu Energi (Energy) Medan Magnet Induktor?", "id": "U = ½ * L * I²." },
  { "en": "Apa Itu Rangkaian RLC (Resistor Induktor Kapasitor)?", "id": "Rangkaian Mengandung R, L, C." },
  { "en": "Apa Itu Osilasi (Oscillation) Rangkaian LC Ideal?", "id": "Pertukaran Energi Antara L Dan C." },
  { "en": "Apa Frekuensi Osilasi (Oscillation) Alami LC?", "id": "ω₀ = 1 / √(LC)." },
  { "en": "Apa Itu Osilasi Teredam (Damped Oscillation) RLC?", "id": "Osilasi Energi Hilang Akibat R." },
  { "en": "Apa Itu Persamaan Maxwell (Maxwell's Equations) Bentuk Diferensial?", "id": "Empat Persamaan Diferensial Medan E, B." },
  { "en": "Apa Itu Persamaan Gelombang (Wave Equation) Elektromagnetik?", "id": "Persamaan Deskripsi Perambatan Gelombang EM." },
  { "en": "Apa Sumber Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Muatan Listrik Yang Dipercepat." },
  { "en": "Apa Itu Polarisasi (Polarization) Gelombang EM?", "id": "Orientasi Getaran Medan Listrik (E)." },
  { "en": "Apa Itu Vektor Poynting (Poynting Vector) (S)?", "id": "Laju Aliran Energi Gelombang EM." },
  { "en": "Apa Rumus Vektor Poynting (Poynting Vector)?", "id": "S = (1/μ₀) * (E x B)." },
  { "en": "Apa Itu Momentum (Momentum) Gelombang EM?", "id": "Gelombang EM Membawa Momentum." },
  { "en": "Apa Itu Tekanan Radiasi (Radiation Pressure)?", "id": "Tekanan Diberikan Gelombang EM Permukaan." },
  { "en": "Apa Itu Radiasi Dipol (Dipole Radiation)?", "id": "Pancaran Gelombang EM Dipol Osilasi." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Struktur Meradiasikan/Menerima Gelombang EM Efisien." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Pemandu Perambatan Gelombang EM." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance)?", "id": "Rasio Tegangan, Arus Gelombang Berjalan." },
  { "en": "Apa Itu Refleksi (Reflection) Gelombang Saluran?", "id": "Pantulan Akibat Ketidakcocokan Impedansi." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient)?", "id": "Ukuran Rasio Gelombang Pantul, Datang." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Gelombang Berdiri Akibat Refleksi." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Meminimalkan Refleksi Maksimalkan Transfer Daya." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Pipa Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu Mode Propagasi (Propagation Mode) Pemandu?", "id": "Pola Medan EM Dapat Merambat." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) Pemandu?", "id": "Frekuensi Minimum Mode Dapat Merambat." },
  { "en": "Apa Itu Resonator Rongga (Cavity Resonator)?", "id": "Rongga Logam Resonansi Gelombang EM." },
  { "en": "Apa Itu Optika (Optics)?", "id": "Studi Perilaku Cahaya." },
  { "en": "Apa Itu Prinsip Huygens (Huygens' Principle)?", "id": "Setiap Titik Muka Gelombang Sumber Gelombang Baru." },
  { "en": "Apa Itu Interferensi (Interference)?", "id": "Superposisi Gelombang Hasilkan Pola." },
  { "en": "Apa Itu Difraksi (Diffraction)?", "id": "Pelenturan Gelombang Sekitar Hambatan." },
  { "en": "Apa Itu Kisi Difraksi (Diffraction Grating)?", "id": "Alat Memisahkan Cahaya Menjadi Spektrum." },
  { "en": "Apa Itu Polarisasi (Polarization) Cahaya?", "id": "Orientasi Getaran Medan Listrik Cahaya." },
  { "en": "Apa Itu Filter Polarisasi (Polarizing Filter)?", "id": "Melewatkan Cahaya Polarisasi Tertentu." },
  { "en": "Apa Itu Hukum Malus (Malus's Law)?", "id": "Intensitas Cahaya Lewat Polarisator Ideal." },
  { "en": "Apa Itu Sudut Brewster (Brewster's Angle)?", "id": "Sudut Pantul Polarisasi Sempurna." },
  { "en": "Apa Itu Hamburan Rayleigh (Rayleigh Scattering)?", "id": "Hamburan Cahaya Partikel Kecil (Langit Biru)." },
  { "en": "Apa Itu Hamburan Mie (Mie Scattering)?", "id": "Hamburan Cahaya Partikel Lebih Besar (Awan)." },
  { "en": "Apa Itu Laser (Laser)?", "id": "Sumber Cahaya Koheren, Monokromatik." },
  { "en": "Prinsip Dasar Laser (Laser)?", "id": "Emisi Terstimulasi (Stimulated Emission)." },
  { "en": "Apa Itu Inversi Populasi (Population Inversion)?", "id": "Kondisi Perlu Aktivitas Laser." },
  { "en": "Apa Itu Resonator Optik (Optical Resonator)?", "id": "Rongga Cermin Penguat Cahaya Laser." },
  { "en": "Apa Itu Holografi (Holography)?", "id": "Perekaman Gambar Tiga Dimensi." },
  { "en": "Apa Itu Serat Optik (Optical Fiber)?", "id": "Pemandu Cahaya Transmisi Data." },
  { "en": "Prinsip Serat Optik (Optical Fiber)?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Fotonik (Photonics)?", "id": "Teknologi Berbasis Foton (Cahaya)." },
  { "en": "Apa Itu Optoelektronika (Optoelectronics)?", "id": "Perangkat Elektronik Interaksi Cahaya." },
  { "en": "Apa Itu Relativitas Khusus (Special Relativity)?", "id": "Teori Ruang, Waktu, Gerak Einstein." },
  { "en": "Apa Dua Postulat Relativitas Khusus?", "id": "Prinsip Relativitas, Konstanta Kecepatan Cahaya." },
  { "en": "Apa Itu Dilatasi Waktu (Time Dilation)?", "id": "Waktu Melambat Bagi Pengamat Bergerak." },
  { "en": "Apa Itu Kontraksi Panjang (Length Contraction)?", "id": "Panjang Tampak Memendek Arah Gerak." },
  { "en": "Apa Itu Kesetaraan Massa-Energi (Mass-Energy Equivalence)?", "id": "E = mc²." },
  { "en": "Apa Itu Relativitas Umum (General Relativity)?", "id": "Teori Gravitasi Einstein (Kelengkungan Ruangwaktu)." },
  { "en": "Apa Itu Prinsip Ekuivalensi (Equivalence Principle)?", "id": "Gravitasi Setara Percepatan." },
  { "en": "Apa Itu Kelengkungan Ruangwaktu (Spacetime Curvature)?", "id": "Penyebab Efek Gravitasi." },
  { "en": "Apa Itu Lensa Gravitasi (Gravitational Lensing)?", "id": "Pembelokan Cahaya Medan Gravitasi Kuat." },
  { "en": "Apa Itu Gelombang Gravitasi (Gravitational Wave)?", "id": "Riak Dalam Ruangwaktu." },
  { "en": "Apa Itu Mekanika Kuantum (Quantum Mechanics)?", "id": "Fisika Mengatur Dunia Skala Atom." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Besaran Fisik Hanya Miliki Nilai Diskrit." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel (Wave-Particle Duality)?", "id": "Partikel Menunjukkan Sifat Gelombang, Sebaliknya." },
  { "en": "Apa Itu Fungsi Gelombang (Wave Function)?", "id": "Deskripsi Probabilitas Keadaan Kuantum." },
  { "en": "Apa Itu Persamaan Schrödinger (Schrödinger Equation)?", "id": "Persamaan Evolusi Fungsi Gelombang." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Batas Akurasi Pengukuran Pasangan Variabel." },
  { "en": "Apa Itu Spin (Spin) Partikel?", "id": "Momentum Sudut Intrinsik Partikel Kuantum." },
  { "en": "Apa Itu Statistik Fermi-Dirac (Fermi-Dirac Statistics)?", "id": "Statistik Partikel Spin Setengah Bulat (Fermion)." },
  { "en": "Apa Itu Fermion (Fermion)?", "id": "Partikel Tunduk Prinsip Eksklusi Pauli." },
  { "en": "Contoh Fermion (Fermion)?", "id": "Elektron, Proton, Neutron." },
  { "en": "Apa Itu Statistik Bose-Einstein (Bose-Einstein Statistics)?", "id": "Statistik Partikel Spin Bulat (Boson)." },
  { "en": "Apa Itu Boson (Boson)?", "id": "Partikel Tidak Tunduk Prinsip Eksklusi." },
  { "en": "Contoh Boson (Boson)?", "id": "Foton, Gluon, Boson Higgs." },
  { "en": "Apa Itu Kondensat Bose-Einstein (Bose-Einstein Condensate)?", "id": "Wujud Materi Suhu Sangat Rendah (Boson)." },
  { "en": "Apa Itu Terowongan Kuantum (Quantum Tunneling)?", "id": "Partikel Menembus Penghalang Potensial Klasik." },
  { "en": "Aplikasi Terowongan Kuantum (Quantum Tunneling)?", "id": "Dioda Tunnel, Mikroskop Pemindai Terowongan." },
  { "en": "Apa Itu Mikroskop Pemindai Terowongan (STM)?", "id": "Scanning Tunneling Microscope (STM)." },
  { "en": "Apa Kepanjangan STM (Scanning Tunneling Microscope)?", "id": "Mikroskop Pemindai Terowongan." },
  { "en": "Apa Fungsi STM (Scanning Tunneling Microscope)?", "id": "Pencitraan Permukaan Skala Atom." },
  { "en": "Apa Itu Keterikatan Kuantum (Quantum Entanglement)?", "id": "Korelasi Kuantum Antar Partikel Terpisah." },
  { "en": "Apa Itu Komputasi Kuantum (Quantum Computing)?", "id": "Komputasi Memanfaatkan Fenomena Kuantum." },
  { "en": "Apa Itu Qubit (Quantum Bit)?", "id": "Unit Dasar Informasi Kuantum." },
  { "en": "Apa Itu Superposisi (Superposition) Qubit?", "id": "Qubit Bisa Dalam Keadaan 0, 1 Bersamaan." },
  { "en": "Apa Itu Gerbang Kuantum (Quantum Gate)?", "id": "Operasi Logika Pada Qubit." },
  { "en": "Apa Itu Algoritma Kuantum (Quantum Algorithm)?", "id": "Algoritma Dijalankan Komputer Kuantum." },
  { "en": "Apa Itu Dekokerensi (Decoherence) Kuantum?", "id": "Hilangnya Sifat Kuantum Akibat Lingkungan." },
  { "en": "Tantangan Utama Komputasi Kuantum?", "id": "Mengatasi Dekokerensi, Skalabilitas Qubit." },
  { "en": "Apa Itu Elektrodinamika Kuantum (QED)?", "id": "Quantum Electrodynamics (QED)." },
  { "en": "Apa Kepanjangan QED (Quantum Electrodynamics)?", "id": "Elektrodinamika Kuantum." },
  { "en": "Apa Yang Dijelaskan QED (Quantum Electrodynamics)?", "id": "Interaksi Cahaya Dengan Materi Kuantum." },
  { "en": "Apa Itu Kromodinamika Kuantum (QCD)?", "id": "Quantum Chromodynamics (QCD)." },
  { "en": "Apa Kepanjangan QCD (Quantum Chromodynamics)?", "id": "Kromodinamika Kuantum." },
  { "en": "Apa Yang Dijelaskan QCD (Quantum Chromodynamics)?", "id": "Interaksi Kuat Antara Quark, Gluon." },
  { "en": "Apa Itu Teori Medan Kuantum (QFT)?", "id": "Quantum Field Theory (QFT)." },
  { "en": "Apa Kepanjangan QFT (Quantum Field Theory)?", "id": "Teori Medan Kuantum." },
  { "en": "Apa Kerangka Dasar QFT (Quantum Field Theory)?", "id": "Menggabungkan Mekanika Kuantum, Relativitas Khusus." },
  { "en": "Apa Itu Model Standar (Standard Model) Fisika Partikel?", "id": "Teori Partikel Dasar, Interaksi Fundamental." },
  { "en": "Gaya Apa Yang Tidak Termasuk Model Standar?", "id": "Gaya Gravitasi." },
  { "en": "Apa Itu Teori Segalanya (Theory of Everything)?", "id": "Teori Hipotetis Satukan Semua Gaya." },
  { "en": "Apa Itu Teori String (String Theory)?", "id": "Kandidat Teori Segalanya (Partikel String)." },
  { "en": "Apa Itu Dimensi Ekstra (Extra Dimensions)?", "id": "Dimensi Ruang Tambahan Teori String." },
  { "en": "Apa Itu Supersimetri (Supersymmetry) (SUSY)?", "id": "Simetri Hipotetis Antara Fermion, Boson." },
  { "en": "Apa Kepanjangan SUSY (Supersymmetry)?", "id": "Supersimetri." },
  { "en": "Apa Itu Fisika Benda Terkondensasi (Condensed Matter)?", "id": "Studi Sifat Fisik Materi Padat, Cair." },
  { "en": "Apa Itu Struktur Kristal (Crystal Structure)?", "id": "Susunan Atom Teratur Dalam Padatan." },
  { "en": "Apa Itu Kisi Bravais (Bravais Lattice)?", "id": "Empat Belas Tipe Dasar Kisi Kristal." },
  { "en": "Apa Itu Sel Satuan (Unit Cell)?", "id": "Unit Berulang Terkecil Kisi Kristal." },
  { "en": "Apa Itu Difraksi Sinar-X (X-Ray Diffraction) (XRD)?", "id": "Teknik Penentuan Struktur Kristal." },
  { "en": "Apa Hukum Bragg (Bragg's Law)?", "id": "Kondisi Interferensi Konstruktif Difraksi Sinar-X." },
  { "en": "Apa Itu Cacat (Defect) Kristal?", "id": "Ketidaksempurnaan Susunan Atom Kristal." },
  { "en": "Jenis Cacat (Defect) Titik Kristal?", "id": "Kekosongan (Vacancy), Interstisial, Substitusi." },
  { "en": "Apa Itu Dislokasi (Dislocation) Kristal?", "id": "Cacat Garis Dalam Struktur Kristal." },
  { "en": "Apa Pengaruh Dislokasi (Dislocation)?", "id": "Mempengaruhi Kekuatan Mekanis Material." },
  { "en": "Apa Itu Batas Butir (Grain Boundary)?", "id": "Antarmuka Antara Kristalit Berbeda Orientasi." },
  { "en": "Apa Itu Fonon (Phonon)?", "id": "Kuantum Getaran Kisi Kristal." },
  { "en": "Apa Peran Fonon (Phonon)?", "id": "Konduktivitas Termal, Hamburan Elektron." },
  { "en": "Apa Itu Konduktivitas Termal (Thermal Conductivity)?", "id": "Kemampuan Bahan Hantarkan Panas." },
  { "en": "Apa Itu Konduktivitas Listrik (Electrical Conductivity)?", "id": "Kemampuan Bahan Hantarkan Listrik." },
  { "en": "Apa Itu Model Elektron Bebas (Free Electron Model)?", "id": "Model Sederhana Elektron Konduksi Logam." },
  { "en": "Apa Itu Permukaan Fermi (Fermi Surface)?", "id": "Batas Energi Elektron Terisi Logam." },
  { "en": "Apa Itu Struktur Pita (Band Structure) Elektronik?", "id": "Rentang Energi Elektron Diperbolehkan Padatan." },
  { "en": "Apa Menentukan Sifat Konduktor/Isolator/Semikonduktor?", "id": "Struktur Pita Dan Celah Energi." },
  { "en": "Apa Itu Massa Efektif (Effective Mass) Elektron?", "id": "Massa Semu Elektron Dalam Kristal." },
  { "en": "Apa Itu Hole (Lubang)?", "id": "Ketiadaan Elektron Pita Valensi (Pembawa Positif)." },
  { "en": "Apa Itu Semikonduktor Celah Langsung (Direct Gap)?", "id": "Rekombinasi Efisien Hasilkan Cahaya (LED)." },
  { "en": "Apa Itu Semikonduktor Celah Tak Langsung (Indirect Gap)?", "id": "Rekombinasi Libatkan Fonon (Silikon)." },
  { "en": "Apa Itu Magnetisme (Magnetism)?", "id": "Fenomena Medan Magnet." },
  { "en": "Apa Itu Diamagnetisme (Diamagnetism)?", "id": "Bahan Sedikit Menolak Medan Magnet." },
  { "en": "Apa Itu Paramagnetisme (Paramagnetism)?", "id": "Bahan Sedikit Tertarik Medan Magnet." },
  { "en": "Apa Itu Feromagnetisme (Ferromagnetism)?", "id": "Bahan Magnet Kuat (Besi, Nikel)." },
  { "en": "Apa Itu Domain Magnetik (Magnetic Domain)?", "id": "Daerah Magnetisasi Searah Feromagnetik." },
  { "en": "Apa Itu Titik Curie (Curie Temperature)?", "id": "Suhu Hilangnya Sifat Feromagnetik." },
  { "en": "Apa Itu Antiferomagnetisme (Antiferromagnetism)?", "id": "Momen Magnetik Tetangga Berlawanan Arah." },
  { "en": "Apa Itu Ferrimagnetisme (Ferrimagnetism)?", "id": "Momen Magnetik Tetangga Berlawanan, Beda Magnitudo." },
  { "en": "Contoh Bahan Ferrimagnetik (Ferrimagnetic Material)?", "id": "Ferit (Ferrite) (Inti Induktor)." },
  { "en": "Apa Itu Magnetoresistansi (Magnetoresistance)?", "id": "Perubahan Resistansi Bahan Medan Magnet." },
  { "en": "Apa Itu Efek Spin Hall (Spin Hall Effect)?", "id": "Arus Spin Dihasilkan Arus Muatan." },
  { "en": "Apa Itu Spintronics (Spintronics)?", "id": "Elektronika Memanfaatkan Spin Elektron." },
  { "en": "Aplikasi Spintronics (Spintronics)?", "id": "Memori MRAM (Magnetoresistive RAM), Sensor Magnetik." },
  { "en": "Apa Kepanjangan MRAM (Magnetoresistive RAM)?", "id": "RAM Magnetoresistif." },
  { "en": "Apa Itu Superfluida (Superfluidity)?", "id": "Fluida Mengalir Tanpa Viskositas (Helium Cair)." },
  { "en": "Apa Itu Kristal Cair (Liquid Crystal)?", "id": "Fasa Antara Cairan, Kristal Padat." },
  { "en": "Aplikasi Kristal Cair (Liquid Crystal)?", "id": "Tampilan LCD (Liquid Crystal Display)." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Molekul Rantai Panjang Unit Berulang." },
  { "en": "Apa Itu Plastik (Plastic)?", "id": "Polimer Sintetis Dapat Dibentuk." },
  { "en": "Apa Itu Termoplastik (Thermoplastic)?", "id": "Plastik Melunak Dipanaskan, Mengeras Didinginkan." },
  { "en": "Apa Itu Termoset (Thermosetting Plastic)?", "id": "Plastik Mengeras Permanen Setelah Dipanaskan." },
  { "en": "Apa Itu Elastomer (Elastomer)?", "id": "Polimer Elastis (Karet)." },
  { "en": "Apa Itu Bahan Komposit (Composite Material)?", "id": "Gabungan Dua Bahan Berbeda Sifat Unggul." },
  { "en": "Contoh Bahan Komposit (Composite Material)?", "id": "Fiberglass (Serat Kaca + Resin)." },
  { "en": "Apa Itu Keramik (Ceramic)?", "id": "Bahan Padat Anorganik Non-Logam." },
  { "en": "Sifat Umum Keramik (Ceramic)?", "id": "Keras, Rapuh, Tahan Panas, Isolator." },
  { "en": "Contoh Keramik (Ceramic) Teknik?", "id": "Alumina (Al₂O₃), Zirkonia (ZrO₂)." },
  { "en": "Apa Itu Gelas (Glass)?", "id": "Padatan Amorf (Non-Kristalin)." },
  { "en": "Apa Itu Logam (Metal)?", "id": "Unsur Konduktif, Mengkilap, Dapat Ditempa." },
  { "en": "Apa Itu Paduan (Alloy)?", "id": "Campuran Logam Dengan Unsur Lain." },
  { "en": "Contoh Paduan (Alloy)?", "id": "Baja (Besi + Karbon), Kuningan (Tembaga + Seng)." },
  { "en": "Apa Itu Korosi (Corrosion)?", "id": "Degradasi Logam Akibat Reaksi Kimia." },
  { "en": "Apa Itu Pencegahan Korosi (Corrosion Prevention)?", "id": "Pelapisan, Proteksi Katodik, Inhibitor." },
  { "en": "Apa Itu Pengujian Material (Materials Testing)?", "id": "Menentukan Sifat Mekanis, Fisik Material." },
  { "en": "Contoh Pengujian Mekanis (Mechanical Testing)?", "id": "Uji Tarik, Uji Keras, Uji Impak." },
  { "en": "Apa Itu Uji Tarik (Tensile Test)?", "id": "Mengukur Kekuatan, Keuletan Material." },
  { "en": "Apa Itu Uji Keras (Hardness Test)?", "id": "Mengukur Ketahanan Material Terhadap Goresan." },
  { "en": "Apa Itu Uji Impak (Impact Test)?", "id": "Mengukur Ketahanan Material Beban Kejut." },
  { "en": "Apa Itu Pengujian Tak Merusak (NDT)?", "id": "Non-Destructive Testing (NDT)." },
  { "en": "Apa Kepanjangan NDT (Non-Destructive Testing)?", "id": "Pengujian Tak Merusak." },
  { "en": "Contoh Metode NDT (Non-Destructive Testing)?", "id": "Ultrasonik, Radiografi, Partikel Magnetik." },
  { "en": "Apa Itu Radiografi (Radiography) Industri?", "id": "Penggunaan Sinar-X/Gama Deteksi Cacat Internal." },
  { "en": "Apa Itu Pengujian Ultrasonik (Ultrasonic Testing)?", "id": "Menggunakan Gelombang Suara Frekuensi Tinggi." },
  { "en": "Apa Fungsi Pengujian Ultrasonik (Ultrasonic Testing)?", "id": "Deteksi Cacat Internal, Ukur Ketebalan." },
  { "en": "Apa Itu Pengujian Partikel Magnetik (Magnetic Particle Testing)?", "id": "Deteksi Cacat Permukaan Bahan Feromagnetik." },
  { "en": "Apa Itu Pengujian Penetran Cair (Liquid Penetrant Testing)?", "id": "Deteksi Cacat Permukaan Terbuka." },
  { "en": "Apa Itu Analisis Kegagalan (Failure Analysis)?", "id": "Investigasi Penyebab Kegagalan Komponen/Sistem." },
  { "en": "Apa Itu Fraktografi (Fractography)?", "id": "Studi Permukaan Patahan Material." },
  { "en": "Apa Itu Metalografi (Metallography)?", "id": "Studi Struktur Mikro Logam, Paduan." },
  { "en": "Apa Itu Mikroskop Optik (Optical Microscope)?", "id": "Mikroskop Menggunakan Cahaya Tampak." },
  { "en": "Apa Itu Perbesaran (Magnification)?", "id": "Rasio Ukuran Gambar Terhadap Objek." },
  { "en": "Apa Itu Resolusi (Resolution) Mikroskop?", "id": "Kemampuan Membedakan Dua Titik Terdekat." },
  { "en": "Apa Itu Mikroskop Elektron Pemindai (SEM)?", "id": "Gambar Permukaan Resolusi Tinggi (Elektron)." },
  { "en": "Apa Itu Mikroskop Elektron Transmisi (TEM)?", "id": "Gambar Struktur Internal Tipis (Elektron)." },
  { "en": "Apa Itu Mikroskop Gaya Atom (AFM)?", "id": "Pencitraan Topografi Permukaan Skala Atom." },
  { "en": "Apa Itu Spektroskopi (Spectroscopy)?", "id": "Studi Interaksi Materi, Radiasi Elektromagnetik." },
  { "en": "Apa Itu Spektroskopi Serapan Atom (AAS)?", "id": "Atomic Absorption Spectroscopy (AAS)." },
  { "en": "Apa Kepanjangan AAS (Atomic Absorption Spectroscopy)?", "id": "Spektroskopi Serapan Atom." },
  { "en": "Apa Fungsi AAS (Atomic Absorption Spectroscopy)?", "id": "Analisis Konsentrasi Unsur Logam." },
  { "en": "Apa Itu Spektroskopi Emisi Atom (AES)?", "id": "Atomic Emission Spectroscopy (AES)." },
  { "en": "Apa Kepanjangan AES (Atomic Emission Spectroscopy)?", "id": "Spektroskopi Emisi Atom." },
  { "en": "Apa Itu Spektroskopi Massa (Mass Spectrometry)?", "id": "Mengukur Rasio Massa Terhadap Muatan Ion." },
  { "en": "Apa Itu Spektroskopi Inframerah (Infrared Spectroscopy) (IR)?", "id": "Analisis Getaran Molekul (Gugus Fungsi)." },
  { "en": "Apa Itu Spektroskopi UV-Vis (UV-Vis Spectroscopy)?", "id": "Analisis Serapan Cahaya UV/Tampak (Elektronik)." },
  { "en": "Apa Itu Spektroskopi Raman (Raman Spectroscopy)?", "id": "Analisis Hamburan Cahaya Inelastis (Vibrasi)." },
  { "en": "Apa Itu Kromatografi (Chromatography)?", "id": "Teknik Pemisahan Campuran Kimia." },
  { "en": "Apa Itu Kromatografi Gas (Gas Chromatography) (GC)?", "id": "Pemisahan Senyawa Volatil Fasa Gas." },
  { "en": "Apa Itu Kromatografi Cair Kinerja Tinggi (HPLC)?", "id": "High Performance Liquid Chromatography (HPLC)." },
  { "en": "Apa Kepanjangan HPLC (High Performance Liquid Chromatography)?", "id": "Kromatografi Cair Kinerja Tinggi." },
  { "en": "Apa Fasa Gerak (Mobile Phase) Kromatografi?", "id": "Fluida Pembawa Sampel Lewati Fasa Diam." },
  { "en": "Apa Fasa Diam (Stationary Phase) Kromatografi?", "id": "Material Padat/Cair Menahan Komponen." },
  { "en": "Apa Itu Waktu Retensi (Retention Time)?", "id": "Waktu Komponen Lewati Kolom Kromatografi." },
  { "en": "Apa Itu Kromatogram (Chromatogram)?", "id": "Grafik Hasil Pemisahan Kromatografi." },
  { "en": "Apa Itu Elektroforesis (Electrophoresis)?", "id": "Pemisahan Molekul Bermuatan Medan Listrik." },
  { "en": "Dimana Elektroforesis (Electrophoresis) Digunakan?", "id": "Pemisahan DNA, RNA, Protein." },
  { "en": "Apa Itu Reaksi Berantai Polimerase (PCR)?", "id": "Polymerase Chain Reaction (PCR)." },
  { "en": "Apa Kepanjangan PCR (Polymerase Chain Reaction)?", "id": "Reaksi Berantai Polimerase." },
  { "en": "Apa Fungsi PCR (Polymerase Chain Reaction)?", "id": "Menggandakan Segmen DNA Secara Eksponensial." },
  { "en": "Apa Itu Sekuensing DNA (DNA Sequencing)?", "id": "Menentukan Urutan Basa Nukleotida DNA." },
  { "en": "Apa Itu Rekayasa Genetika (Genetic Engineering)?", "id": "Manipulasi Materi Genetik Organisme." },
  { "en": "Apa Itu DNA (Deoxyribonucleic Acid) Rekombinan?", "id": "DNA (Deoxyribonucleic Acid) Gabungan Sumber Berbeda." },
  { "en": "Apa Itu Kloning (Cloning)?", "id": "Membuat Salinan Identik Gen/Organisme." },
  { "en": "Apa Itu Terapi Gen (Gene Therapy)?", "id": "Memperbaiki Kelainan Genetik." },
  { "en": "Apa Itu Sel Punca (Stem Cell)?", "id": "Sel Belum Terdiferensiasi Potensi Beragam." },
  { "en": "Apa Itu Bioinformatika (Bioinformatics)?", "id": "Aplikasi Komputasi Analisis Data Biologis." },
  { "en": "Apa Itu Bioteknologi (Biotechnology)?", "id": "Pemanfaatan Sistem Biologis Teknologi." },
  { "en": "Apa Itu Fermentasi (Fermentation)?", "id": "Proses Metabolisme Hasilkan Energi Tanpa Oksigen." },
  { "en": "Apa Itu Antibiotik (Antibiotic)?", "id": "Senyawa Membunuh/Menghambat Pertumbuhan Bakteri." },
  { "en": "Apa Itu Vaksin (Vaccine)?", "id": "Mempersiapkan Sistem Imun Lawan Penyakit." },
  { "en": "Apa Itu Virus (Virus)?", "id": "Agen Infeksius Mikroskopis (Replikasi Sel Inang)." },
  { "en": "Apa Itu Bakteri (Bacteria)?", "id": "Mikroorganisme Prokariotik Uniseluler." },
  { "en": "Apa Itu Jamur (Fungi)?", "id": "Organisme Eukariotik (Ragi, Kapang, Jamur)." },
  { "en": "Apa Itu Protista (Protist)?", "id": "Organisme Eukariotik Sederhana (Amoeba, Alga)." },
  { "en": "Apa Itu Ekologi (Ecology)?", "id": "Studi Interaksi Organisme, Lingkungan." },
  { "en": "Apa Itu Ekosistem (Ecosystem)?", "id": "Komunitas Organisme Interaksi Lingkungan Fisik." },
  { "en": "Apa Itu Rantai Makanan (Food Chain)?", "id": "Urutan Transfer Energi Antar Organisme." },
  { "en": "Apa Itu Jaring Makanan (Food Web)?", "id": "Interkoneksi Rantai Makanan Ekosistem." },
  { "en": "Apa Itu Produsen (Producer) Ekosistem?", "id": "Organisme Membuat Makanan Sendiri (Tumbuhan)." },
  { "en": "Apa Itu Konsumen (Consumer) Ekosistem?", "id": "Organisme Makan Organisme Lain." },
  { "en": "Apa Itu Dekomposer (Decomposer) Ekosistem?", "id": "Organisme Mengurai Bahan Organik Mati." },
  { "en": "Apa Itu Siklus Biogeokimia (Biogeochemical Cycle)?", "id": "Perputaran Unsur Kimia Ekosistem." },
  { "en": "Contoh Siklus Biogeokimia (Biogeochemical Cycle)?", "id": "Siklus Air, Karbon, Nitrogen." },
  { "en": "Apa Itu Pemanasan Global (Global Warming)?", "id": "Peningkatan Suhu Rata-Rata Bumi." },
  { "en": "Penyebab Utama Pemanasan Global (Global Warming)?", "id": "Emisi Gas Rumah Kaca Manusia." },
  { "en": "Apa Itu Gas Rumah Kaca (Greenhouse Gas)?", "id": "Gas Menyerap Radiasi Inframerah Atmosfer." },
  { "en": "Contoh Gas Rumah Kaca (Greenhouse Gas)?", "id": "Karbon Dioksida (CO₂), Metana (CH₄)." },
  { "en": "Apa Itu Perubahan Iklim (Climate Change)?", "id": "Perubahan Jangka Panjang Pola Cuaca." },
  { "en": "Apa Dampak Perubahan Iklim (Climate Change)?", "id": "Naik Muka Air Laut, Cuaca Ekstrem." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Energi Sumber Tak Habis Alami." },
  { "en": "Apa Itu Efisiensi Energi (Energy Efficiency)?", "id": "Menggunakan Energi Lebih Sedikit Hasil Sama." },
  { "en": "Apa Itu Pembangunan Berkelanjutan (Sustainable Development)?", "id": "Pembangunan Penuhi Kebutuhan Tanpa Kompromi." },
  { "en": "Apa Itu Jejak Karbon (Carbon Footprint)?", "id": "Total Emisi Gas Rumah Kaca Individu/Aktivitas." },
  { "en": "Apa Itu Daur Ulang (Recycling)?", "id": "Mengolah Kembali Bahan Bekas Jadi Baru." },
  { "en": "Apa Itu Pengurangan Limbah (Waste Reduction)?", "id": "Meminimalkan Jumlah Sampah Dihasilkan." },
  { "en": "Apa Itu Penggunaan Kembali (Reuse)?", "id": "Menggunakan Kembali Barang Tanpa Pengolahan." },
  { "en": "Apa Itu Kompos (Composting)?", "id": "Penguraian Bahan Organik Jadi Pupuk." },
  { "en": "Apa Itu Konservasi Air (Water Conservation)?", "id": "Penggunaan Air Secara Bijak." },
  { "en": "Apa Itu Polusi Udara (Air Pollution)?", "id": "Kontaminasi Udara Zat Berbahaya." },
  { "en": "Apa Itu Polusi Air (Water Pollution)?", "id": "Kontaminasi Sumber Air Zat Berbahaya." },
  { "en": "Apa Itu Polusi Tanah (Soil Pollution)?", "id": "Kontaminasi Tanah Zat Berbahaya." },
  { "en": "Apa Itu Keanekaragaman Hayati (Biodiversity)?", "id": "Variasi Kehidupan Di Bumi." },
  { "en": "Mengapa Keanekaragaman Hayati (Biodiversity) Penting?", "id": "Menjaga Keseimbangan Ekosistem, Jasa Lingkungan." },
  { "en": "Apa Itu Deforestasi (Deforestation)?", "id": "Penggundulan Hutan Skala Besar." },
  { "en": "Apa Itu Spesies Terancam Punah (Endangered Species)?", "id": "Spesies Berisiko Tinggi Punah." },
  { "en": "Apa Itu Konservasi (Conservation)?", "id": "Perlindungan, Pengelolaan Sumber Daya Alam." },
  { "en": "Apa Itu Energi Nuklir (Nuclear Energy)?", "id": "Energi Dilepaskan Reaksi Nuklir." },
  { "en": "Apa Keuntungan Energi Nuklir (Nuclear Energy)?", "id": "Emisi Karbon Rendah, Kepadatan Energi Tinggi." },
  { "en": "Apa Kerugian Energi Nuklir (Nuclear Energy)?", "id": "Limbah Radioaktif, Risiko Kecelakaan." },
  { "en": "Apa Itu Geologi (Geology)?", "id": "Studi Bumi Padat, Batuan." },
  { "en": "Apa Tiga Jenis Batuan Utama?", "id": "Beku, Sedimen, Metamorf." },
  { "en": "Apa Itu Batuan Beku (Igneous Rock)?", "id": "Terbentuk Pendinginan Magma/Lava." },
  { "en": "Apa Itu Batuan Sedimen (Sedimentary Rock)?", "id": "Terbentuk Endapan Terkompaksi." },
  { "en": "Apa Itu Batuan Metamorf (Metamorphic Rock)?", "id": "Terbentuk Perubahan Batuan Akibat Panas/Tekanan." },
  { "en": "Apa Itu Lempeng Tektonik (Tectonic Plate)?", "id": "Lempengan Kaku Litosfer Bumi Bergerak." },
  { "en": "Apa Itu Gempa Bumi (Earthquake)?", "id": "Getaran Permukaan Bumi Akibat Pelepasan Energi." },
  { "en": "Apa Itu Gunung Berapi (Volcano)?", "id": "Bukaaan Kerak Bumi Keluarkan Lava, Abu." },
  { "en": "Apa Itu Erosi (Erosion)?", "id": "Proses Pengikisan Tanah/Batuan Oleh Alam." },
  { "en": "Apa Itu Pelapukan (Weathering)?", "id": "Proses Pemecahan Batuan Permukaan Bumi." },
  { "en": "Apa Itu Fosil (Fossil)?", "id": "Sisa Organisme Masa Lalu Terawetkan." },
  { "en": "Apa Itu Skala Waktu Geologi (Geologic Time Scale)?", "id": "Sistem Penanggalan Sejarah Bumi." },
  { "en": "Apa Itu Astronomi (Astronomy)?", "id": "Studi Benda Langit, Alam Semesta." },
  { "en": "Apa Itu Tata Surya (Solar System)?", "id": "Matahari Dan Benda Langit Mengorbitnya." },
  { "en": "Apa Benda Pusat Tata Surya?", "id": "Matahari (Sun)." },
  { "en": "Berapa Jumlah Planet Tata Surya?", "id": "Delapan (Merkurius Hingga Neptunus)." },
  { "en": "Urutan Planet Dari Matahari?", "id": "Merkurius, Venus, Bumi, Mars, Jupiter, Saturnus, Uranus, Neptunus." },
  { "en": "Apa Itu Planet Katai (Dwarf Planet)?", "id": "Benda Langit Mirip Planet (Pluto)." },
  { "en": "Apa Itu Satelit (Satellite) Alami?", "id": "Benda Langit Mengorbit Planet (Bulan)." },
  { "en": "Apa Itu Asteroid (Asteroid)?", "id": "Benda Berbatu Kecil Antara Mars, Jupiter." },
  { "en": "Apa Itu Sabuk Asteroid (Asteroid Belt)?", "id": "Wilayah Utama Orbit Asteroid." },
  { "en": "Apa Itu Komet (Comet)?", "id": "Benda Es, Debu Orbit Elips Panjang." },
  { "en": "Apa Itu Ekor (Tail) Komet?", "id": "Gas, Debu Dilepas Komet Dekat Matahari." },
  { "en": "Apa Itu Meteoroid (Meteoroid)?", "id": "Batuan Kecil Melayang Luar Angkasa." },
  { "en": "Apa Itu Meteor (Meteor)?", "id": "Cahaya Meteoroid Terbakar Atmosfer." },
  { "en": "Apa Itu Meteorit (Meteorite)?", "id": "Sisa Meteoroid Sampai Permukaan Bumi." },
  { "en": "Apa Itu Bintang (Star)?", "id": "Bola Plasma Raksasa Hasilkan Cahaya." },
  { "en": "Apa Sumber Energi Bintang (Star)?", "id": "Reaksi Fusi Nuklir Di Intinya." },
  { "en": "Apa Itu Galaksi (Galaxy)?", "id": "Kumpulan Miliaran Bintang, Gas, Debu." },
  { "en": "Apa Nama Galaksi (Galaxy) Kita?", "id": "Bima Sakti (Milky Way)." },
  { "en": "Apa Itu Lubang Hitam (Black Hole)?", "id": "Wilayah Gravitasi Sangat Kuat." },
  { "en": "Apa Itu Nebula (Nebula)?", "id": "Awan Gas, Debu Antarbintang." },
  { "en": "Apa Itu Tahun Cahaya (Light Year)?", "id": "Jarak Ditempuh Cahaya Satu Tahun." },
  { "en": "Apa Itu Satuan Astronomi (AU)?", "id": "Jarak Rata-Rata Bumi Ke Matahari." },
  { "en": "Apa Itu Teleskop (Telescope)?", "id": "Alat Mengamati Benda Langit Jauh." },
  { "en": "Apa Itu Rotasi (Rotation) Bumi?", "id": "Perputaran Bumi Pada Porosnya (Hari)." },
  { "en": "Apa Itu Revolusi (Revolution) Bumi?", "id": "Perputaran Bumi Mengelilingi Matahari (Tahun)." },
  { "en": "Apa Penyebab Pergantian Musim?", "id": "Kemiringan Sumbu Rotasi Bumi." },
  { "en": "Apa Itu Gerhana Matahari (Solar Eclipse)?", "id": "Bulan Menutupi Matahari Dari Bumi." },
  { "en": "Apa Itu Gerhana Bulan (Lunar Eclipse)?", "id": "Bumi Menghalangi Cahaya Matahari Ke Bulan." },
  { "en": "Apa Itu Fase Bulan (Moon Phase)?", "id": "Perubahan Penampakan Bulan Dari Bumi." },
  { "en": "Apa Itu Pasang Surut (Tides)?", "id": "Naik Turunnya Permukaan Laut Gravitasi." },
  { "en": "Apa Penyebab Utama Pasang Surut?", "id": "Gaya Gravitasi Bulan Dan Matahari." },
  { "en": "Apa Itu Meteorologi (Meteorology)?", "id": "Studi Atmosfer, Cuaca, Iklim." },
  { "en": "Apa Itu Atmosfer (Atmosphere)?", "id": "Lapisan Gas Menyelimuti Bumi." },
  { "en": "Apa Komposisi Utama Atmosfer Bumi?", "id": "Nitrogen (N₂) Dan Oksigen (O₂)." },
  { "en": "Sebutkan Lapisan Atmosfer (Atmosphere) Utama?", "id": "Troposfer, Stratosfer, Mesosfer, Termosfer." },
  { "en": "Di Lapisan Mana Cuaca Terjadi?", "id": "Troposfer (Troposphere)." },
  { "en": "Di Lapisan Mana Lapisan Ozon Berada?", "id": "Stratosfer (Stratosphere)." },
  { "en": "Apa Fungsi Lapisan Ozon (Ozone Layer)?", "id": "Menyerap Radiasi Ultraviolet (UV) Berbahaya." },
  { "en": "Apa Itu Cuaca (Weather)?", "id": "Kondisi Atmosfer Sesaat Suatu Tempat." },
  { "en": "Apa Itu Iklim (Climate)?", "id": "Pola Cuaca Rata-Rata Jangka Panjang." },
  { "en": "Apa Unsur-Unsur Cuaca (Weather Element)?", "id": "Suhu, Tekanan, Angin, Kelembaban, Awan." },
  { "en": "Apa Itu Suhu (Temperature) Udara?", "id": "Ukuran Derajat Panas Dingin Udara." },
  { "en": "Apa Alat Ukur Suhu (Temperature)?", "id": "Termometer (Thermometer)." },
  { "en": "Apa Itu Tekanan (Pressure) Udara?", "id": "Gaya Diberikan Udara Per Luas." },
  { "en": "Apa Alat Ukur Tekanan (Pressure) Udara?", "id": "Barometer (Barometer)." },
  { "en": "Apa Itu Angin (Wind)?", "id": "Pergerakan Udara Akibat Perbedaan Tekanan." },
  { "en": "Dari Mana Arah Angin (Wind) Bertiup?", "id": "Dari Tekanan Tinggi Ke Rendah." },
  { "en": "Apa Alat Ukur Kecepatan Angin (Wind Speed)?", "id": "Anemometer (Anemometer)." },
  { "en": "Apa Alat Ukur Arah Angin (Wind Direction)?", "id": "Penunjuk Arah Angin (Wind Vane)." },
  { "en": "Apa Itu Kelembaban (Humidity) Udara?", "id": "Jumlah Uap Air Dalam Udara." },
  { "en": "Apa Itu Kelembaban Relatif (Relative Humidity)?", "id": "Persentase Uap Air Dibanding Maksimum." },
  { "en": "Apa Alat Ukur Kelembaban (Humidity)?", "id": "Higrometer (Hygrometer)." },
  { "en": "Apa Itu Awan (Cloud)?", "id": "Kumpulan Tetes Air/Kristal Es Atmosfer." },
  { "en": "Bagaimana Awan (Cloud) Terbentuk?", "id": "Pendinginan Udara Lembab Hingga Jenuh." },
  { "en": "Apa Itu Presipitasi (Precipitation)?", "id": "Air Jatuh Dari Awan Ke Bumi." },
  { "en": "Bentuk Presipitasi (Precipitation) Umum?", "id": "Hujan, Salju, Hujan Es." },
  { "en": "Apa Itu Kabut (Fog)?", "id": "Awan Yang Menyentuh Permukaan Bumi." },
  { "en": "Apa Itu Embun (Dew)?", "id": "Uap Air Mengembun Permukaan Dingin." },
  { "en": "Apa Itu Siklus Air (Water Cycle)?", "id": "Pergerakan Air Terus Menerus Di Bumi." },
  { "en": "Tahapan Utama Siklus Air (Water Cycle)?", "id": "Evaporasi, Kondensasi, Presipitasi." },
  { "en": "Apa Itu Evaporasi (Evaporation)?", "id": "Perubahan Air Cair Menjadi Uap." },
  { "en": "Apa Itu Kondensasi (Condensation)?", "id": "Perubahan Uap Air Menjadi Cair." },
  { "en": "Apa Itu Transpirasi (Transpiration)?", "id": "Penguapan Air Dari Tumbuhan." },
  { "en": "Apa Itu Front (Front) Cuaca?", "id": "Batas Antara Dua Massa Udara Berbeda." },
  { "en": "Apa Itu Front Dingin (Cold Front)?", "id": "Massa Udara Dingin Gantikan Hangat." },
  { "en": "Apa Itu Front Hangat (Warm Front)?", "id": "Massa Udara Hangat Gantikan Dingin." },
  { "en": "Apa Itu Sistem Tekanan Tinggi (High Pressure)?", "id": "Area Tekanan Udara Lebih Tinggi (Cerah)." },
  { "en": "Apa Itu Sistem Tekanan Rendah (Low Pressure)?", "id": "Area Tekanan Udara Lebih Rendah (Berawan/Hujan)." },
  { "en": "Apa Itu Badai Petir (Thunderstorm)?", "id": "Badai Hujan Disertai Petir, Kilat." },
  { "en": "Apa Itu Petir (Lightning)?", "id": "Pelepasan Listrik Statis Besar Atmosfer." },
  { "en": "Apa Itu Guntur (Thunder)?", "id": "Suara Akibat Ekspansi Udara Cepat Petir." },
  { "en": "Apa Itu Tornado (Tornado)?", "id": "Kolom Udara Berputar Kencang Sentuh Tanah." },
  { "en": "Apa Itu Badai Tropis (Hurricane/Typhoon/Cyclone)?", "id": "Sistem Badai Besar Lautan Tropis." },
  { "en": "Apa Itu El Niño (El Niño)?", "id": "Pemanasan Periodik Permukaan Laut Pasifik." },
  { "en": "Apa Itu La Niña (La Niña)?", "id": "Pendinginan Periodik Permukaan Laut Pasifik." },
  { "en": "Apa Itu Perubahan Iklim (Climate Change)?", "id": "Perubahan Pola Iklim Jangka Panjang." },
  { "en": "Apa Itu Oseanografi (Oceanography)?", "id": "Studi Tentang Lautan." },
  { "en": "Apa Itu Arus Laut (Ocean Current)?", "id": "Pergerakan Massa Air Laut Terarah." },
  { "en": "Penyebab Arus Laut (Ocean Current)?", "id": "Angin, Perbedaan Suhu, Salinitas." },
  { "en": "Apa Itu Salinitas (Salinity)?", "id": "Kadar Garam Dalam Air Laut." },
  { "en": "Apa Itu Gelombang Laut (Ocean Wave)?", "id": "Gerakan Naik Turun Permukaan Laut." },
  { "en": "Penyebab Utama Gelombang Laut (Ocean Wave)?", "id": "Angin Yang Bertiup." },
  { "en": "Apa Itu Tsunami (Tsunami)?", "id": "Gelombang Laut Besar Akibat Gempa Dasar Laut." },
  { "en": "Apa Itu Terumbu Karang (Coral Reef)?", "id": "Ekosistem Bawah Laut Dibangun Karang." },
  { "en": "Apa Itu Biologi (Biology)?", "id": "Studi Tentang Kehidupan." },
  { "en": "Apa Ciri-Ciri Makhluk Hidup?", "id": "Bernapas, Bergerak, Makan, Tumbuh, Berkembang Biak." },
  { "en": "Apa Itu Sel (Cell)?", "id": "Unit Dasar Kehidupan." },
  { "en": "Apa Dua Jenis Sel Utama?", "id": "Prokariotik Dan Eukariotik." },
  { "en": "Apa Itu Sel Prokariotik (Prokaryotic Cell)?", "id": "Sel Tanpa Inti Sejati (Bakteri)." },
  { "en": "Apa Itu Sel Eukariotik (Eukaryotic Cell)?", "id": "Sel Dengan Inti Sejati (Hewan, Tumbuhan)." },
  { "en": "Apa Itu DNA (Deoxyribonucleic Acid)?", "id": "Molekul Pembawa Informasi Genetik." },
  { "en": "Apa Itu Gen (Gene)?", "id": "Segmen DNA Kode Protein Tertentu." },
  { "en": "Apa Itu Kromosom (Chromosome)?", "id": "Struktur DNA Terorganisir Dalam Sel." },
  { "en": "Apa Itu Mitosis (Mitosis)?", "id": "Pembelahan Sel Hasilkan Sel Identik." },
  { "en": "Apa Itu Meiosis (Meiosis)?", "id": "Pembelahan Sel Hasilkan Sel Kelamin." },
  { "en": "Apa Itu Genetika (Genetics)?", "id": "Studi Pewarisan Sifat." },
  { "en": "Siapa Bapak Genetika (Father of Genetics)?", "id": "Gregor Mendel." },
  { "en": "Apa Itu Alel (Allele)?", "id": "Variasi Bentuk Suatu Gen." },
  { "en": "Apa Itu Genotipe (Genotype)?", "id": "Komposisi Genetik Suatu Organisme." },
  { "en": "Apa Itu Fenotipe (Phenotype)?", "id": "Sifat Fisik Teramati Organisme." },
  { "en": "Apa Itu Evolusi (Evolution)?", "id": "Perubahan Sifat Terwariskan Populasi." },
  { "en": "Apa Itu Seleksi Alam (Natural Selection)?", "id": "Mekanisme Utama Evolusi (Survival Fittest)." },
  { "en": "Siapa Pencetus Teori Seleksi Alam?", "id": "Charles Darwin." },
  { "en": "Apa Itu Taksonomi (Taxonomy)?", "id": "Ilmu Klasifikasi Makhluk Hidup." },
  { "en": "Siapa Bapak Taksonomi (Father of Taxonomy) Modern?", "id": "Carolus Linnaeus." },
  { "en": "Apa Itu Sistem Klasifikasi Binomial Nomenklatur?", "id": "Pemberian Nama Ilmiah Dua Kata (Genus, Spesies)." },
  { "en": "Urutan Tingkatan Taksonomi (Taxonomic Rank) Utama?", "id": "Kingdom, Filum, Kelas, Ordo, Famili, Genus, Spesies." },
  { "en": "Apa Lima Kingdom (Kingdom) Utama?", "id": "Monera, Protista, Fungi, Plantae, Animalia." },
  { "en": "Apa Ciri Kingdom Monera (Monera)?", "id": "Prokariotik (Bakteri, Arkea)." },
  { "en": "Apa Ciri Kingdom Protista (Protista)?", "id": "Eukariotik Uniseluler/Koloni Sederhana." },
  { "en": "Apa Ciri Kingdom Fungi (Fungi)?", "id": "Eukariotik Heterotrof Dinding Kitin." },
  { "en": "Apa Ciri Kingdom Plantae (Plantae)?", "id": "Eukariotik Autotrof Fotosintesis." },
  { "en": "Apa Ciri Kingdom Animalia (Animalia)?", "id": "Eukariotik Heterotrof Multiseluler Bergerak." },
  { "en": "Apa Itu Fotosintesis (Photosynthesis)?", "id": "Proses Tumbuhan Buat Makanan Cahaya." },
  { "en": "Apa Bahan Baku Fotosintesis (Photosynthesis)?", "id": "Karbon Dioksida (CO₂), Air (H₂O)." },
  { "en": "Apa Hasil Fotosintesis (Photosynthesis)?", "id": "Glukosa (Gula), Oksigen (O₂)." },
  { "en": "Dimana Fotosintesis (Photosynthesis) Terjadi?", "id": "Kloroplas (Chloroplast) Dalam Daun." },
  { "en": "Apa Pigmen Hijau Fotosintesis (Photosynthesis)?", "id": "Klorofil (Chlorophyll)." },
  { "en": "Apa Itu Respirasi Seluler (Cellular Respiration)?", "id": "Proses Hasilkan Energi (ATP) Dari Makanan." },
  { "en": "Dimana Respirasi Seluler (Cellular Respiration) Eukariotik Terjadi?", "id": "Sitoplasma Dan Mitokondria." },
  { "en": "Apa Molekul Energi Utama Sel?", "id": "ATP (Adenosine Triphosphate)." },
  { "en": "Apa Itu Anatomi (Anatomy)?", "id": "Studi Struktur Tubuh Organisme." },
  { "en": "Apa Itu Fisiologi (Physiology)?", "id": "Studi Fungsi Tubuh Organisme." },
  { "en": "Apa Sistem Organ (Organ System) Utama Manusia?", "id": "Pencernaan, Pernapasan, Sirkulasi, Saraf, dll." },
  { "en": "Apa Fungsi Sistem Pencernaan (Digestive System)?", "id": "Mencerna Makanan, Menyerap Nutrisi." },
  { "en": "Apa Fungsi Sistem Pernapasan (Respiratory System)?", "id": "Pertukaran Gas Oksigen, Karbon Dioksida." },
  { "en": "Apa Fungsi Sistem Sirkulasi (Circulatory System)?", "id": "Mengedarkan Darah, Nutrisi, Oksigen." },
  { "en": "Apa Fungsi Sistem Saraf (Nervous System)?", "id": "Mengatur Koordinasi Tubuh, Respon Rangsang." },
  { "en": "Apa Bagian Utama Sistem Saraf?", "id": "Otak, Sumsum Tulang Belakang, Saraf." },
  { "en": "Apa Itu Sel Saraf (Neuron)?", "id": "Unit Dasar Sistem Saraf." },
  { "en": "Apa Itu Sinapsis (Synapse)?", "id": "Celah Antara Dua Sel Saraf." },
  { "en": "Apa Itu Neurotransmiter (Neurotransmitter)?", "id": "Zat Kimia Pembawa Sinyal Sinapsis." },
  { "en": "Apa Fungsi Sistem Rangka (Skeletal System)?", "id": "Menyangga Tubuh, Melindungi Organ." },
  { "en": "Apa Fungsi Sistem Otot (Muscular System)?", "id": "Memungkinkan Pergerakan Tubuh." },
  { "en": "Apa Fungsi Sistem Endokrin (Endocrine System)?", "id": "Menghasilkan Hormon Pengatur Fungsi Tubuh." },
  { "en": "Apa Itu Hormon (Hormone)?", "id": "Pesan Kimiawi Diedarkan Lewat Darah." },
  { "en": "Apa Fungsi Sistem Imun (Immune System)?", "id": "Melindungi Tubuh Dari Penyakit." },
  { "en": "Apa Itu Antibodi (Antibody)?", "id": "Protein Sistem Imun Lawan Patogen." },
  { "en": "Apa Itu Patogen (Pathogen)?", "id": "Agen Penyebab Penyakit (Virus, Bakteri)." },
  { "en": "Apa Fungsi Sistem Ekskresi (Excretory System)?", "id": "Mengeluarkan Sisa Metabolisme." },
  { "en": "Organ Ekskresi (Excretory Organ) Utama Manusia?", "id": "Ginjal, Kulit, Paru-Paru." },
  { "en": "Apa Fungsi Ginjal (Kidney)?", "id": "Menyaring Darah, Menghasilkan Urin." },
  { "en": "Apa Itu Kimia (Chemistry)?", "id": "Studi Materi Dan Perubahannya." },
  { "en": "Apa Itu Atom (Atom)?", "id": "Partikel Dasar Penyusun Unsur Kimia." },
  { "en": "Apa Partikel Subatomik (Subatomic Particle) Utama?", "id": "Proton, Neutron, Elektron." },
  { "en": "Apa Itu Unsur (Element)?", "id": "Zat Murni Terdiri Satu Jenis Atom." },
  { "en": "Apa Itu Senyawa (Compound)?", "id": "Zat Terbentuk Ikatan Kimia Unsur Berbeda." },
  { "en": "Apa Itu Molekul (Molecule)?", "id": "Partikel Terkecil Senyawa/Unsur Stabil." },
  { "en": "Apa Itu Ikatan Kimia (Chemical Bond)?", "id": "Gaya Menyatukan Atom Dalam Molekul." },
  { "en": "Apa Itu Ikatan Ionik (Ionic Bond)?", "id": "Ikatan Akibat Tarik Elektrostatis Ion." },
  { "en": "Apa Itu Ikatan Kovalen (Covalent Bond)?", "id": "Ikatan Akibat Pemakaian Bersama Elektron." },
  { "en": "Apa Itu Tabel Periodik (Periodic Table)?", "id": "Susunan Unsur Berdasarkan Nomor Atom." },
  { "en": "Apa Itu Golongan (Group) Tabel Periodik?", "id": "Kolom Vertikal (Sifat Mirip)." },
  { "en": "Apa Itu Periode (Period) Tabel Periodik?", "id": "Baris Horizontal (Kulit Elektron)." },
  { "en": "Apa Itu Reaksi Kimia (Chemical Reaction)?", "id": "Proses Perubahan Zat Menjadi Zat Baru." },
  { "en": "Apa Itu Reaktan (Reactant)?", "id": "Zat Awal Reaksi Kimia." },
  { "en": "Apa Itu Produk (Product)?", "id": "Zat Hasil Reaksi Kimia." },
  { "en": "Apa Itu Hukum Kekekalan Massa?", "id": "Massa Total Sebelum, Sesudah Reaksi Sama." },
  { "en": "Apa Itu Mol (Mole)?", "id": "Satuan Jumlah Zat Kimia." },
  { "en": "Apa Itu Bilangan Avogadro (Avogadro's Number)?", "id": "Jumlah Partikel Dalam Satu Mol." },
  { "en": "Apa Itu Larutan (Solution)?", "id": "Campuran Homogen Zat Terlarut, Pelarut." },
  { "en": "Apa Itu Pelarut (Solvent)?", "id": "Komponen Jumlah Terbesar Larutan." },
  { "en": "Apa Itu Zat Terlarut (Solute)?", "id": "Komponen Jumlah Lebih Kecil Larutan." },
  { "en": "Apa Itu Konsentrasi (Concentration)?", "id": "Ukuran Jumlah Zat Terlarut Larutan." },
  { "en": "Apa Itu Molaritas (Molarity)?", "id": "Mol Zat Terlarut Per Liter Larutan." },
  { "en": "Apa Itu Asam (Acid)?", "id": "Zat Hasilkan Ion H+ Dalam Air." },
  { "en": "Apa Itu Basa (Base)?", "id": "Zat Hasilkan Ion OH- Dalam Air." },
  { "en": "Apa Itu Skala pH (pH Scale)?", "id": "Ukuran Keasaman/Kebasaan Larutan." },
  { "en": "Rentang Skala pH (pH Scale)?", "id": "Nol Hingga Empat Belas." },
  { "en": "pH (pH Value) Asam?", "id": "Kurang Dari Tujuh (< 7)." },
  { "en": "pH (pH Value) Basa?", "id": "Lebih Dari Tujuh (> 7)." },
  { "en": "pH (pH Value) Netral?", "id": "Tujuh (7)." },
  { "en": "Apa Itu Reaksi Netralisasi (Neutralization)?", "id": "Reaksi Asam Basa Hasilkan Garam, Air." },
  { "en": "Apa Itu Reaksi Redoks (Redox Reaction)?", "id": "Reaksi Transfer Elektron." },
  { "en": "Apa Itu Oksidasi (Oxidation)?", "id": "Kehilangan Elektron (Biloks Naik)." },
  { "en": "Apa Itu Reduksi (Reduction)?", "id": "Penerimaan Elektron (Biloks Turun)." },
  { "en": "Apa Itu Kimia Organik (Organic Chemistry)?", "id": "Studi Senyawa Karbon." },
  { "en": "Apa Itu Hidrokarbon (Hydrocarbon)?", "id": "Senyawa Hanya Mengandung Karbon, Hidrogen." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Molekul Raksasa Unit Berulang." },
  { "en": "Contoh Polimer (Polymer) Alami?", "id": "Protein, DNA, Selulosa." },
  { "en": "Contoh Polimer (Polymer) Sintetis?", "id": "Plastik, Nilon, Karet Sintetis." },
  { "en": "Apa Itu Biokimia (Biochemistry)?", "id": "Kimia Proses Makhluk Hidup." },
  { "en": "Apa Empat Makromolekul Utama Kehidupan?", "id": "Karbohidrat, Lipid, Protein, Asam Nukleat." },
  { "en": "Apa Itu Fisika (Physics)?", "id": "Studi Materi, Energi, Interaksinya." },
  { "en": "Apa Itu Mekanika (Mechanics)?", "id": "Studi Gerak Benda." },
  { "en": "Apa Itu Kinematika (Kinematics)?", "id": "Deskripsi Gerak Tanpa Penyebab." },
  { "en": "Apa Itu Dinamika (Dynamics)?", "id": "Studi Penyebab Gerak (Gaya)." },
  { "en": "Apa Itu Hukum Gerak Newton (Newton's Laws)?", "id": "Tiga Hukum Dasar Mekanika Klasik." },
  { "en": "Apa Itu Massa (Mass)?", "id": "Ukuran Inersia Benda." },
  { "en": "Apa Itu Berat (Weight)?", "id": "Gaya Gravitasi Pada Massa." },
  { "en": "Apa Itu Energi (Energy)?", "id": "Kemampuan Melakukan Kerja." },
  { "en": "Apa Itu Energi Kinetik (Kinetic Energy)?", "id": "Energi Akibat Gerak." },
  { "en": "Apa Itu Energi Potensial (Potential Energy)?", "id": "Energi Akibat Posisi/Konfigurasi." },
  { "en": "Apa Itu Hukum Kekekalan Energi (Conservation of Energy)?", "id": "Energi Total Sistem Tertutup Konstan." },
  { "en": "Apa Itu Termodinamika (Thermodynamics)?", "id": "Studi Panas, Kerja, Energi." },
  { "en": "Apa Hukum Pertama Termodinamika?", "id": "Hukum Kekekalan Energi." },
  { "en": "Apa Hukum Kedua Termodinamika?", "id": "Entropi (Entropy) Semesta Meningkat." },
  { "en": "Apa Itu Entropi (Entropy)?", "id": "Ukuran Ketidakteraturan Sistem." },
  { "en": "Apa Itu Optika (Optics)?", "id": "Studi Cahaya Dan Perilakunya." },
  { "en": "Apa Itu Gelombang (Wave)?", "id": "Gangguan Merambat Membawa Energi." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Relativitas (Relativity)?", "id": "Teori Einstein Ruang, Waktu, Gravitasi." },
  { "en": "Apa Itu Mekanika Kuantum (Quantum Mechanics)?", "id": "Fisika Skala Sangat Kecil (Atom)." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Jaringan Listrik Pembangkit Hingga Konsumen." },
  { "en": "Apa Tiga Komponen Utama Sistem Tenaga?", "id": "Pembangkitan, Transmisi, Distribusi." },
  { "en": "Apa Fungsi Pembangkitan (Generation)?", "id": "Menghasilkan Energi Listrik." },
  { "en": "Apa Fungsi Transmisi (Transmission)?", "id": "Menyalurkan Listrik Jarak Jauh (HV)." },
  { "en": "Apa Fungsi Distribusi (Distribution)?", "id": "Menyalurkan Listrik Ke Konsumen (LV/MV)." },
  { "en": "Mengapa Tegangan Transmisi Sangat Tinggi?", "id": "Mengurangi Rugi-Rugi Daya (I²R)." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Transformasi Tegangan, Switching." },
  { "en": "Apa Fungsi Transformator (Transformer) Gardu Induk?", "id": "Menaikkan Atau Menurunkan Tegangan." },
  { "en": "Apa Itu Peralatan Hubung Bagi (Switchgear)?", "id": "Peralatan Pemutus, Pemisah, Proteksi." },
  { "en": "Apa Itu Pemutus Tenaga (Circuit Breaker)?", "id": "Saklar Pemutus Arus Gangguan Otomatis." },
  { "en": "Apa Itu Pemisah (Disconnector/Isolator)?", "id": "Saklar Pemisah Tanpa Beban Arus." },
  { "en": "Apa Fungsi Pemisah (Disconnector)?", "id": "Mengisolasi Peralatan Untuk Pemeliharaan Aman." },
  { "en": "Apa Itu Busbar (Busbar)?", "id": "Konduktor Penghubung Utama Di Gardu." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Perangkat Pendeteksi Gangguan Sistem Tenaga." },
  { "en": "Apa Respon Rele Proteksi (Protection Relay)?", "id": "Memberi Sinyal Trip Ke Pemutus." },
  { "en": "Apa Itu Trafo Arus (Current Transformer) (CT)?", "id": "Sensor Arus Tinggi Untuk Pengukuran/Proteksi." },
  { "en": "Apa Itu Trafo Tegangan (Potential Transformer) (PT)?", "id": "Sensor Tegangan Tinggi Untuk Pengukuran/Proteksi." },
  { "en": "Mengapa Sekunder CT (Current Transformer) Tak Boleh Terbuka?", "id": "Tegangan Sangat Tinggi Berbahaya Muncul." },
  { "en": "Apa Itu Beban (Load) Sistem Tenaga?", "id": "Total Daya Dikonsumsi Pelanggan." },
  { "en": "Apa Itu Beban Puncak (Peak Load)?", "id": "Permintaan Daya Tertinggi Sistem." },
  { "en": "Apa Itu Beban Dasar (Base Load)?", "id": "Permintaan Daya Minimum Konstan." },
  { "en": "Pembangkit Apa Cocok Beban Dasar (Base Load)?", "id": "PLTU (Pembangkit Listrik Tenaga Uap), PLTN (Pembangkit Listrik Tenaga Nuklir)." },
  { "en": "Pembangkit Apa Cocok Beban Puncak (Peak Load)?", "id": "PLTG (Pembangkit Listrik Tenaga Gas), PLTD (Pembangkit Listrik Tenaga Diesel)." },
  { "en": "Apa Itu Faktor Beban (Load Factor)?", "id": "Rasio Beban Rata-Rata Terhadap Puncak." },
  { "en": "Apa Itu Stabilitas (Stability) Sistem Tenaga?", "id": "Kemampuan Sistem Kembali Normal Pasca Gangguan." },
  { "en": "Jenis Stabilitas (Stability) Sistem Tenaga Utama?", "id": "Sudut Rotor, Tegangan, Frekuensi." },
  { "en": "Apa Itu Stabilitas Sudut Rotor (Rotor Angle)?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas Tegangan (Voltage Stability)?", "id": "Kemampuan Sistem Jaga Level Tegangan." },
  { "en": "Apa Itu Stabilitas Frekuensi (Frequency Stability)?", "id": "Kemampuan Sistem Jaga Frekuensi Konstan." },
  { "en": "Apa Penyebab Utama Ketidakstabilan Frekuensi?", "id": "Ketidakseimbangan Pembangkitan Dan Beban." },
  { "en": "Apa Itu Gangguan (Fault) Sistem Tenaga?", "id": "Kondisi Abnormal (Hubung Singkat, Open)." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Kontak Arus Rendah Antar Konduktor." },
  { "en": "Apa Akibat Hubung Singkat (Short Circuit)?", "id": "Arus Sangat Tinggi, Tegangan Turun." },
  { "en": "Apa Itu Gangguan Simetris (Symmetrical Fault)?", "id": "Gangguan Tiga Fasa Seimbang." },
  { "en": "Apa Itu Gangguan Asimetris (Asymmetrical Fault)?", "id": "Gangguan Tidak Seimbang (Satu Fasa)." },
  { "en": "Metode Analisis Gangguan Asimetris (Asymmetrical Fault)?", "id": "Komponen Simetris (Symmetrical Components)." },
  { "en": "Apa Itu Komponen Simetris (Symmetrical Components)?", "id": "Urutan Positif, Negatif, Nol." },
  { "en": "Apa Itu Urutan Positif (Positive Sequence)?", "id": "Komponen Sistem Tiga Fasa Normal." },
  { "en": "Apa Itu Urutan Negatif (Negative Sequence)?", "id": "Komponen Akibat Ketidakseimbangan (Putaran Terbalik)." },
  { "en": "Apa Itu Urutan Nol (Zero Sequence)?", "id": "Komponen Sefasa (Hanya Ada Gangguan Tanah)." },
  { "en": "Apa Itu Grounding (Pembumian) Sistem Tenaga?", "id": "Menghubungkan Titik Netral Ke Tanah." },
  { "en": "Apa Tujuan Grounding (Pembumian) Sistem?", "id": "Keamanan, Stabilitas Tegangan, Deteksi Gangguan." },
  { "en": "Jenis Grounding (Pembumian) Sistem Utama?", "id": "Solid, Resistan, Reaktans, Tak Dibumikan." },
  { "en": "Apa Itu Sistem Solidly Grounded (Dibumikan Solid)?", "id": "Netral Langsung Terhubung Ke Tanah." },
  { "en": "Apa Keuntungan Solidly Grounded (Dibumikan Solid)?", "id": "Tegangan Lebih Rendah Saat Gangguan Tanah." },
  { "en": "Apa Kerugian Solidly Grounded (Dibumikan Solid)?", "id": "Arus Gangguan Tanah Sangat Tinggi." },
  { "en": "Apa Itu Sistem Resistance Grounded (Dibumikan Resistan)?", "id": "Netral Ke Tanah Lewat Resistor." },
  { "en": "Apa Keuntungan Resistance Grounded (Dibumikan Resistan)?", "id": "Membatasi Arus Gangguan Tanah." },
  { "en": "Apa Itu Sistem Ungrounded (Tak Dibumikan) (IT)?", "id": "Netral Tidak Sengaja Dihubungkan Tanah." },
  { "en": "Apa Keuntungan Sistem Ungrounded (Tak Dibumikan)?", "id": "Operasi Bisa Lanjut Gangguan Tanah Pertama." },
  { "en": "Apa Kerugian Sistem Ungrounded (Tak Dibumikan)?", "id": "Tegangan Lebih Tinggi, Sulit Deteksi Gangguan." },
  { "en": "Apa Itu Arester Surja (Surge Arrester)?", "id": "Pelindung Peralatan Dari Tegangan Lebih Surja." },
  { "en": "Apa Penyebab Surja (Surge) Tegangan?", "id": "Sambaran Petir Atau Operasi Switching." },
  { "en": "Bagaimana Arester Surja (Surge Arrester) Bekerja?", "id": "Menyalurkan Arus Surja Ke Tanah." },
  { "en": "Apa Itu Koordinasi Isolasi (Insulation Coordination)?", "id": "Menyesuaikan Kekuatan Isolasi, Level Proteksi." },
  { "en": "Apa Itu Level Isolasi Dasar (BIL)?", "id": "Basic Insulation Level (BIL)." },
  { "en": "Apa Kepanjangan BIL (Basic Insulation Level)?", "id": "Level Isolasi Dasar (Impuls Petir)." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Karakteristik Tegangan, Arus, Frekuensi." },
  { "en": "Masalah Kualitas Daya (Power Quality) Umum?", "id": "Sag, Swell, Harmonisa, Transien, Flicker." },
  { "en": "Apa Itu Sag (Sag) Tegangan?", "id": "Penurunan Tegangan Sesaat." },
  { "en": "Apa Itu Swell (Swell) Tegangan?", "id": "Kenaikan Tegangan Sesaat." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Dari Fundamental." },
  { "en": "Apa Penyebab Harmonisa (Harmonics)?", "id": "Beban Non-Linear (Elektronika Daya)." },
  { "en": "Apa Dampak Buruk Harmonisa (Harmonics)?", "id": "Pemanasan Berlebih, Gangguan Peralatan." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Total Distorsi Harmonisa." },
  { "en": "Apa Itu Filter Harmonisa (Harmonic Filter)?", "id": "Mengurangi Atau Menghilangkan Harmonisa." },
  { "en": "Jenis Filter Harmonisa (Harmonic Filter)?", "id": "Pasif (LC) Dan Aktif (Elektronik)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Memperbaiki Faktor Daya Beban." },
  { "en": "Apa Itu Kapasitor Bank (Capacitor Bank)?", "id": "Kumpulan Kapasitor Koreksi Faktor Daya." },
  { "en": "Apa Itu Transien (Transient)?", "id": "Perubahan Tegangan/Arus Sangat Cepat." },
  { "en": "Apa Itu Flicker (Flicker) Tegangan?", "id": "Fluktuasi Tegangan Sebabkan Lampu Berkedip." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Tak Terputus." },
  { "en": "Apa Fungsi Utama UPS (Uninterruptible Power Supply)?", "id": "Memberi Daya Cadangan Saat Padam." },
  { "en": "Apa Itu FACTS (Flexible AC Transmission Systems)?", "id": "Peralatan Elektronika Daya Kontrol Transmisi." },
  { "en": "Apa Tujuan FACTS (Flexible AC Transmission Systems)?", "id": "Meningkatkan Kapasitas, Stabilitas Transmisi." },
  { "en": "Contoh Perangkat FACTS (Flexible AC Transmission Systems)?", "id": "SVC, STATCOM, TCSC, UPFC." },
  { "en": "Apa Itu HVDC (High Voltage Direct Current)?", "id": "Transmisi Daya Arus Searah Tegangan Tinggi." },
  { "en": "Apa Keunggulan HVDC (High Voltage Direct Current)?", "id": "Rugi Rendah Jarak Jauh, Interkoneksi Asinkron." },
  { "en": "Apa Itu Stasiun Konverter (Converter Station) HVDC?", "id": "Mengubah AC Ke DC Dan Sebaliknya." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Energi Sumber Alam Tak Habis." },
  { "en": "Contoh Energi Terbarukan (Renewable Energy)?", "id": "Surya, Angin, Air, Panas Bumi, Biomassa." },
  { "en": "Apa Itu Sel Fotovoltaik (Photovoltaic Cell)?", "id": "Mengubah Cahaya Matahari Langsung Listrik." },
  { "en": "Apa Itu Panel Surya (Solar Panel)?", "id": "Kumpulan Sel Fotovoltaik." },
  { "en": "Apa Itu Inverter (Inverter) Surya?", "id": "Mengubah DC Panel Surya Ke AC." },
  { "en": "Apa Itu MPPT (Maximum Power Point Tracking)?", "id": "Optimasi Daya Output Panel Surya." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Mengubah Energi Kinetik Angin Ke Listrik." },
  { "en": "Apa Komponen Utama Turbin Angin?", "id": "Bilah (Blade), Rotor, Generator, Menara." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air (PLTA)?", "id": "Menggunakan Energi Potensial Air." },
  { "en": "Apa Itu Turbin Air (Hydro Turbine)?", "id": "Mengubah Energi Aliran Air Putaran Mekanis." },
  { "en": "Apa Itu Bendungan (Dam) PLTA?", "id": "Menampung Air, Menciptakan Beda Ketinggian." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Jaringan Listrik Modern Komunikasi Dua Arah." },
  { "en": "Apa Tujuan Smart Grid (Jaringan Cerdas)?", "id": "Efisiensi, Keandalan, Integrasi Terbarukan." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Listrik Komunikasi Otomatis." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Mengelola Pola Konsumsi Listrik." },
  { "en": "Apa Itu Penyimpanan Energi (Energy Storage)?", "id": "Menyimpan Energi Listrik Untuk Nanti." },
  { "en": "Contoh Teknologi Penyimpanan Energi?", "id": "Baterai, Pompa Hidro, Udara Terkompresi." },
  { "en": "Apa Itu BESS (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Baterai." },
  { "en": "Apa Fungsi BESS (Battery Energy Storage System) Jaringan?", "id": "Stabilisasi Frekuensi, Cadangan Daya." },
  { "en": "Apa Itu Kendaraan Listrik (Electric Vehicle) (EV)?", "id": "Kendaraan Penggerak Motor Listrik." },
  { "en": "Apa Itu Pengisian Cerdas (Smart Charging) EV?", "id": "Pengisian Diatur Optimalisasi Jaringan." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Kembalikan Daya Ke Jaringan." },
  { "en": "Apa Itu Efisiensi Energi (Energy Efficiency)?", "id": "Mengurangi Konsumsi Energi Hasil Sama." },
  { "en": "Apa Itu Audit Energi (Energy Audit)?", "id": "Analisis Penggunaan Energi Cari Penghematan." },
  { "en": "Apa Itu Manajemen Energi (Energy Management)?", "id": "Sistem Pemantauan, Kontrol Konsumsi Energi." },
  { "en": "Apa Satuan Dasar Konduktansi Listrik?", "id": "Siemens (S)." },
  { "en": "Apa Satuan Dasar Muatan Listrik?", "id": "Coulomb (C)." },
  { "en": "Apa Satuan Dasar Fluks Magnet?", "id": "Weber (Wb)." },
  { "en": "Apa Satuan Dasar Medan Magnet (B)?", "id": "Tesla (T)." },
  { "en": "Apa Satuan Dasar Energi Listrik?", "id": "Joule (J)." },
  { "en": "Apa Simbol Skematik Sumber Arus AC?", "id": "Lingkaran Dengan Gelombang Sinus." },
  { "en": "Apa Simbol Skematik Sumber Tegangan Terikat?", "id": "Bentuk Belah Ketupat." },
  { "en": "Apa Simbol Skematik Speaker (Pengeras Suara)?", "id": "Bentuk Corong Atau Kotak Speaker." },
  { "en": "Apa Simbol Skematik Mikrofon (Microphone)?", "id": "Lingkaran Dengan Garis Di Dalamnya." },
  { "en": "Apa Simbol Skematik Sekring (Fuse)?", "id": "Persegi Panjang Dengan Garis Melaluinya." },
  { "en": "Hukum Apa Yang Menghubungkan Tegangan, Arus, Resistansi?", "id": "Hukum Ohm (Ohm's Law)." },
  { "en": "Hukum Apa Tentang Arus Di Titik Cabang?", "id": "Hukum Arus Kirchhoff (KCL)." },
  { "en": "Hukum Apa Tentang Tegangan Dalam Loop?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Apa Itu Daya Listrik (Electrical Power)?", "id": "Laju Transfer Energi Listrik." },
  { "en": "Apa Itu Energi Listrik (Electrical Energy)?", "id": "Kemampuan Melakukan Kerja Listrik." },
  { "en": "Komponen Apa Menyimpan Energi Medan Listrik?", "id": "Kapasitor (Capacitor)." },
  { "en": "Komponen Apa Menyimpan Energi Medan Magnet?", "id": "Induktor (Inductor)." },
  { "en": "Komponen Apa Menghambat Aliran Arus?", "id": "Resistor (Resistor)." },
  { "en": "Apa Itu Arus Searah (Direct Current) (DC)?", "id": "Arus Mengalir Satu Arah Tetap." },
  { "en": "Apa Itu Arus Bolak-Balik (Alternating Current) (AC)?", "id": "Arus Berubah Arah Periodik." },
  { "en": "Apa Bentuk Gelombang Listrik PLN?", "id": "Gelombang Sinusoidal (Sine Wave)." },
  { "en": "Apa Itu Frekuensi (Frequency)?", "id": "Jumlah Siklus Per Detik." },
  { "en": "Apa Satuan Frekuensi (Frequency)?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Periode (Period)?", "id": "Waktu Untuk Satu Siklus Lengkap." },
  { "en": "Apa Satuan Periode (Period)?", "id": "Detik (Second)." },
  { "en": "Apa Itu Amplitudo (Amplitude)?", "id": "Nilai Puncak Maksimum Gelombang." },
  { "en": "Apa Itu Nilai RMS (Root Mean Square)?", "id": "Nilai Efektif Setara DC." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total Rangkaian AC." },
  { "en": "Apa Komponen Pembentuk Impedansi (Impedance)?", "id": "Resistansi Dan Reaktansi." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Komponen Kapasitif/Induktif AC." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Daya Nyata Terhadap Daya Semu." },
  { "en": "Nilai Faktor Daya (Power Factor) Ideal?", "id": "Satu (1)." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Penyearah Arus." },
  { "en": "Apa Itu Bias Maju (Forward Bias) Dioda?", "id": "Kondisi Dioda Menghantarkan Arus." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias) Dioda?", "id": "Kondisi Dioda Menghambat Arus." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Semikonduktor Penguat/Saklar." },
  { "en": "Apa Jenis Transistor (Transistor) Utama?", "id": "BJT (Bipolar Junction Transistor) Dan FET (Field Effect Transistor)." },
  { "en": "Transistor (Transistor) Apa Terkontrol Arus?", "id": "BJT (Bipolar Junction Transistor)." },
  { "en": "Transistor (Transistor) Apa Terkontrol Tegangan?", "id": "FET (Field Effect Transistor)." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Meningkatkan Kekuatan Sinyal." },
  { "en": "Apa Itu Penguatan (Gain)?", "id": "Rasio Output Terhadap Input Penguat." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengembalikan Sebagian Output Ke Input." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Menghasilkan Gelombang Periodik." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Melewatkan Frekuensi Tertentu." },
  { "en": "Apa Itu Sirkuit Digital (Digital Circuit)?", "id": "Sirkuit Beroperasi Level Logika." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Sirkuit Digital." },
  { "en": "Apa Dua Level Logika Dasar?", "id": "Tinggi (High/1) Rendah (Low/0)." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Tegangan AC Dengan Induksi." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Mengubah Energi Listrik Jadi Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Mengubah Energi Mekanik Jadi Listrik." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Mudah Hantarkan Listrik." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Sulit Hantarkan Listrik." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Antara Konduktor Isolator." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih (Putus)." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Pelindung Arus Lebih (Reset)." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Dasar." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Tampilkan Bentuk Gelombang." },
  { "en": "Apa Itu Voltmeter (Voltmeter)?", "id": "Mengukur Beda Potensial (Tegangan)." },
  { "en": "Apa Itu Amperemeter (Ammeter)?", "id": "Mengukur Laju Aliran Arus." },
  { "en": "Apa Itu Ohmmeter (Ohmmeter)?", "id": "Mengukur Hambatan Listrik." },
  { "en": "Apa Itu Wattmeter (Wattmeter)?", "id": "Mengukur Daya Listrik Nyata." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah Sinyal AC Menjadi DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah Sinyal DC Menjadi AC." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Semikonduktor Kontrol Daya." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Elemen Pendeteksi Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Elemen Penggerak Hasil Kontrol." },
  { "en": "Apa Itu Loop Tertutup (Closed Loop)?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Logika Industri." },
  { "en": "Apa Itu Medan Magnet (Magnetic Field)?", "id": "Ruang Pengaruh Gaya Magnetik." },
  { "en": "Arus Listrik Menghasilkan Medan Apa?", "id": "Medan Magnet Di Sekelilingnya." },
  { "en": "Apa Itu Induksi Elektromagnetik (Electromagnetic Induction)?", "id": "Pembangkitan GGL Medan Magnet Berubah." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Getaran Medan E Dan B." },
  { "en": "Contoh Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Cahaya Tampak, Gelombang Radio." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Sinyal Informasi Ke Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Memisahkan Sinyal Informasi Dari Pembawa." },
  { "en": "Apa Itu Listrik Statis (Static Electricity)?", "id": "Ketidakseimbangan Muatan Pada Benda." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Hambatan Jenis Spesifik Material." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Kemudahan Material Hantarkan Listrik." },
  { "en": "Apa Itu Bahan Dielektrik (Dielectric Material)?", "id": "Bahan Isolator Dalam Kapasitor." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Medan Listrik Maksimum Sebelum Tembus." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Rangkaian Elektronik Dalam Chip Kecil." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu IC." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Penyimpanan Data Elektronik." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Baca Tulis Akses Acak." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Baca Non-Volatil." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Komunikasi Serial (Serial Communication)?", "id": "Pengiriman Data Bit Satu Per Satu." },
  { "en": "Apa Itu Komunikasi Paralel (Parallel Communication)?", "id": "Pengiriman Data Multi Bit Bersamaan." },
  { "en": "Apa Itu Bus (Bus) Komputer?", "id": "Jalur Komunikasi Berbagi Komponen." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Dalam Jaringan." },
  { "en": "Apa Itu IP Address (Alamat IP)?", "id": "Alamat Unik Perangkat Di Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Keselamatan (Safety) Kerja Listrik?", "id": "Prosedur Aman Bekerja Dengan Listrik." },
  { "en": "Alat Pelindung Diri (APD) Listrik?", "id": "Sarung Tangan Isolasi, Sepatu Safety." },
  { "en": "Apa Itu Lockout/Tagout (LOTO)?", "id": "Prosedur Penguncian Sumber Energi." },
  { "en": "Apa Itu Satuan Tegangan Efektif AC?", "id": "Volt RMS (Root Mean Square)." },
  { "en": "Apa Itu Satuan Arus Efektif AC?", "id": "Ampere RMS (Root Mean Square)." },
  { "en": "Apa Itu Kapasitor Bypass (Bypass Capacitor)?", "id": "Menyaring Noise Frekuensi Tinggi Catu Daya." },
  { "en": "Apa Itu Kopling Kapasitif (Capacitive Coupling)?", "id": "Menghubungkan Sinyal AC, Memblokir DC." },
  { "en": "Apa Fungsi Kapasitor Reservoir (Reservoir Capacitor)?", "id": "Menyimpan Energi Setelah Penyearah." },
  { "en": "Apa Nama Lain Kapasitor Reservoir?", "id": "Kapasitor Penghalus (Smoothing Capacitor)." },
  { "en": "Apa Efek Kapasitansi (Capacitance) Terlalu Besar Filter?", "id": "Arus Puncak Dioda Tinggi." },
  { "en": "Apa Efek Kapasitansi (Capacitance) Terlalu Kecil Filter?", "id": "Riak (Ripple) Tegangan Masih Tinggi." },
  { "en": "Apa Itu Tegangan Riak (Ripple Voltage) Puncak-Ke-Puncak?", "id": "Perbedaan Tegangan Maksimum, Minimum Riak." },
  { "en": "Faktor Apa Mempengaruhi Tegangan Riak (Ripple Voltage)?", "id": "Kapasitansi Filter, Arus Beban." },
  { "en": "Apa Itu Regulator Linear Low Dropout (LDO)?", "id": "Regulator Jatuh Tegangan Sangat Rendah." },
  { "en": "Apa Kepanjangan LDO (Low Dropout)?", "id": "Jatuh Tegangan Rendah." },
  { "en": "Kapan LDO (Low Dropout) Regulator Berguna?", "id": "Saat Tegangan Input Dekat Output." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance) Junction-Ke-Ambient?", "id": "Hambatan Panas Junction Ke Lingkungan." },
  { "en": "Apa Itu Arus Diam (Quiescent Current) Regulator?", "id": "Arus Dikonsumsi Regulator Tanpa Beban." },
  { "en": "Apa Itu Efisiensi (Efficiency) Regulator Linear?", "id": "Efisiensi = (Vout * Iout) / (Vin * Iin)." },
  { "en": "Apa Itu Saklar (Switch) Analog?", "id": "Saklar Elektronik Melewatkan Sinyal Analog." },
  { "en": "Contoh Komponen Saklar (Switch) Analog?", "id": "MOSFET (Metal-Oxide-Semiconductor FET), JFET (Junction Field Effect Transistor), IC Khusus." },
  { "en": "Apa Itu Resistansi On (On-Resistance) Saklar Analog?", "id": "Resistansi Saklar Saat Kondisi On." },
  { "en": "Apa Itu Arus Bocor (Leakage Current) Saklar Analog?", "id": "Arus Kecil Saat Kondisi Off." },
  { "en": "Apa Itu Injeksi Muatan (Charge Injection) Saklar Analog?", "id": "Muatan Kecil Diinjeksikan Saat Switching." },
  { "en": "Apa Itu Multiplexer (Multiplexer) Analog?", "id": "Memilih Satu Input Analog Ke Output." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) Analog?", "id": "Mengarahkan Input Analog Ke Banyak Output." },
  { "en": "Apa Itu Crosstalk (Crosstalk) Saklar Analog?", "id": "Kebocoran Sinyal Antar Kanal." },
  { "en": "Apa Itu Rangkaian Sample And Hold (S&H)?", "id": "Mencuplik, Menahan Tegangan Analog Sesaat." },
  { "en": "Komponen Utama Rangkaian S&H (Sample And Hold)?", "id": "Saklar Analog Dan Kapasitor Penahan." },
  { "en": "Apa Itu Waktu Akuisisi (Acquisition Time) S&H?", "id": "Waktu Kapasitor Ikuti Tegangan Input." },
  { "en": "Apa Itu Waktu Aperture (Aperture Time) S&H?", "id": "Waktu Transisi Dari Sample Ke Hold." },
  { "en": "Apa Itu Droop Rate (Laju Turun) S&H?", "id": "Penurunan Tegangan Tertahan Seiring Waktu." },
  { "en": "Apa Itu Penguat Logaritmik (Logarithmic Amplifier)?", "id": "Output Proporsional Logaritma Tegangan Input." },
  { "en": "Apa Komponen Non-Linear Penguat Logaritmik?", "id": "Dioda Atau Transistor BJT." },
  { "en": "Apa Aplikasi Penguat Logaritmik (Logarithmic Amplifier)?", "id": "Kompresi Sinyal, Pengukuran Daya RF." },
  { "en": "Apa Itu Penguat Antilogaritmik (Antilog Amplifier)?", "id": "Output Proporsional Eksponensial Tegangan Input." },
  { "en": "Apa Itu Penguat Transimpedansi (Transimpedance Amplifier) (TIA)?", "id": "Mengubah Arus Input Menjadi Tegangan Output." },
  { "en": "Apa Kepanjangan TIA (Transimpedance Amplifier)?", "id": "Penguat Transimpedansi." },
  { "en": "Dimana TIA (Transimpedance Amplifier) Digunakan?", "id": "Penguat Sinyal Fotodioda." },
  { "en": "Apa Itu Penguat Diferensial Sepenuhnya (Fully Differential)?", "id": "Memiliki Input Dan Output Diferensial." },
  { "en": "Keuntungan Penguat Diferensial Sepenuhnya (Fully Differential)?", "id": "Penolakan Noise Baik, Rentang Dinamis Lebar." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Ciri Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "CMRR Tinggi, Impedansi Input Tinggi." },
  { "en": "Apa Itu Penguat Isolasi (Isolation Amplifier)?", "id": "Memberikan Isolasi Galvanik Input, Output." },
  { "en": "Metode Isolasi (Isolation) Penguat Isolasi?", "id": "Kapasitif, Magnetik (Trafo), Optik." },
  { "en": "Apa Itu Sumber Arus (Current Source) Howland?", "id": "Rangkaian Op-Amp Sumber Arus Presisi." },
  { "en": "Apa Itu Konverter Tegangan Ke Arus (V-I)?", "id": "Output Arus Proporsional Tegangan Input." },
  { "en": "Apa Itu Konverter Arus Ke Tegangan (I-V)?", "id": "Output Tegangan Proporsional Arus Input (TIA)." },
  { "en": "Apa Itu Noise (Derau) Popcorn (Popcorn Noise)?", "id": "Noise Letupan Acak Frekuensi Rendah." },
  { "en": "Apa Itu Noise (Derau) Avalanche (Avalanche Noise)?", "id": "Noise Dihasilkan Dioda Zener/Avalanche." },
  { "en": "Apa Itu Dithering (Dithering)?", "id": "Menambah Noise Acak Tingkatkan Resolusi Persepsi." },
  { "en": "Dimana Dithering (Dithering) Digunakan?", "id": "Audio Digital, Gambar Digital, ADC." },
  { "en": "Apa Itu Filter Kalman (Kalman Filter)?", "id": "Algoritma Estimasi Keadaan Optimal (Noise)." },
  { "en": "Apa Aplikasi Filter Kalman (Kalman Filter)?", "id": "Navigasi, Pelacakan Target, Kontrol." },
  { "en": "Apa Itu Sistem Linear (Linear System)?", "id": "Output Proporsional Input (Superposisi Berlaku)." },
  { "en": "Apa Itu Sistem Non-Linear (Non-Linear System)?", "id": "Output Tidak Proporsional Input." },
  { "en": "Contoh Non-Linearitas (Non-Linearity) Umum?", "id": "Saturasi, Dead Zone, Histeresis." },
  { "en": "Apa Itu Saturasi (Saturation)?", "id": "Output Terbatas Nilai Maksimum/Minimum." },
  { "en": "Apa Itu Dead Zone (Zona Mati)?", "id": "Rentang Input Tidak Hasilkan Output." },
  { "en": "Apa Itu Histeresis (Hysteresis)?", "id": "Output Tergantung Riwayat Input Sebelumnya." },
  { "en": "Apa Itu Analisis Bidang Fasa (Phase Plane)?", "id": "Metode Grafis Analisis Sistem Non-Linear Orde Dua." },
  { "en": "Apa Itu Fungsi Deskripsi (Describing Function)?", "id": "Aproksimasi Analisis Sistem Non-Linear Frekuensi." },
  { "en": "Apa Itu Kriteria Kestabilan Lingkaran (Circle Criterion)?", "id": "Kriteria Kestabilan Sistem Non-Linear." },
  { "en": "Apa Itu Kriteria Kestabilan Popov (Popov Criterion)?", "id": "Kriteria Kestabilan Absolut Sistem Non-Linear." },
  { "en": "Apa Itu Teori Kestabilan Lyapunov (Lyapunov Stability)?", "id": "Metode Analisis Kestabilan Sistem Dinamis." },
  { "en": "Apa Itu Fungsi Lyapunov (Lyapunov Function)?", "id": "Fungsi Skalar Analisis Kestabilan Lyapunov." },
  { "en": "Apa Itu Kontrol Mode Geser (Sliding Mode Control)?", "id": "Teknik Kontrol Non-Linear Kuat." },
  { "en": "Apa Itu Permukaan Geser (Sliding Surface)?", "id": "Permukaan Target Keadaan Sistem SMC." },
  { "en": "Apa Itu Chattering (Getaran) SMC?", "id": "Osilasi Frekuensi Tinggi Dekat Permukaan Geser." },
  { "en": "Apa Itu Kontrol Adaptif (Adaptive Control)?", "id": "Kontroler Otomatis Sesuaikan Parameter." },
  { "en": "Kapan Kontrol Adaptif (Adaptive Control) Digunakan?", "id": "Saat Parameter Sistem Berubah Waktu." },
  { "en": "Apa Itu Identifikasi Sistem (System Identification)?", "id": "Membangun Model Matematis Dari Data." },
  { "en": "Apa Itu Model Referensi Kontrol Adaptif (MRAC)?", "id": "Model Reference Adaptive Control (MRAC)." },
  { "en": "Apa Kepanjangan MRAC (Model Reference Adaptive Control)?", "id": "Kontrol Adaptif Referensi Model." },
  { "en": "Apa Itu Kontrol Adaptif Self-Tuning (Self-Tuning)?", "id": "Estimasi Parameter Online, Atur Kontroler." },
  { "en": "Apa Itu Kontrol Kuat (Robust Control)?", "id": "Desain Kontroler Tahan Ketidakpastian." },
  { "en": "Apa Itu Ketidakpastian (Uncertainty) Model?", "id": "Perbedaan Model Matematis, Sistem Nyata." },
  { "en": "Apa Itu Kontrol H-Infinity (H-Infinity Control)?", "id": "Metode Desain Kontrol Kuat Optimal." },
  { "en": "Apa Itu Kontrol Mu-Synthesis (Mu-Synthesis)?", "id": "Metode Desain Kontrol Kuat Struktur Ketidakpastian." },
  { "en": "Apa Itu Kontrol Optimal (Optimal Control)?", "id": "Mencari Sinyal Kontrol Minimalkan Biaya." },
  { "en": "Apa Itu Fungsi Biaya (Cost Function)?", "id": "Ukuran Kuantitatif Kinerja Kontrol." },
  { "en": "Apa Itu Linear Quadratic Regulator (LQR)?", "id": "Kontrol Optimal Sistem Linear Biaya Kuadratik." },
  { "en": "Apa Itu Linear Quadratic Gaussian (LQG)?", "id": "Kontrol Optimal (LQR) Dengan Noise Gaussian." },
  { "en": "Apa Kepanjangan LQG (Linear Quadratic Gaussian)?", "id": "Linear Kuadratik Gaussian." },
  { "en": "Apa Itu Model Predictive Control (MPC)?", "id": "Kontrol Gunakan Model Prediksi Optimasi." },
  { "en": "Apa Horizon Prediksi (Prediction Horizon) MPC?", "id": "Jangka Waktu Prediksi Masa Depan." },
  { "en": "Apa Horizon Kontrol (Control Horizon) MPC?", "id": "Jangka Waktu Sinyal Kontrol Dioptimasi." },
  { "en": "Keuntungan MPC (Model Predictive Control)?", "id": "Mampu Menangani Kendala (Constraint)." },
  { "en": "Apa Itu Kontrol Fuzzy (Fuzzy Control)?", "id": "Kontrol Berbasis Logika Fuzzy." },
  { "en": "Kapan Kontrol Fuzzy (Fuzzy Control) Berguna?", "id": "Saat Model Matematis Sulit Didapat." },
  { "en": "Apa Itu Aturan Fuzzy (Fuzzy Rule) (IF-THEN)?", "id": "Basis Pengetahuan Kontroler Fuzzy." },
  { "en": "Apa Itu Fuzzifikasi (Fuzzification)?", "id": "Mengubah Input Crisp Ke Fuzzy Set." },
  { "en": "Apa Itu Mesin Inferensi (Inference Engine) Fuzzy?", "id": "Menerapkan Aturan Fuzzy." },
  { "en": "Apa Itu Defuzzifikasi (Defuzzification)?", "id": "Mengubah Output Fuzzy Ke Crisp." },
  { "en": "Apa Itu Kontrol Jaringan Saraf (Neural Network Control)?", "id": "Menggunakan Jaringan Saraf Tiruan Kontroler." },
  { "en": "Apa Itu Kontrol Neuro-Fuzzy (Neuro-Fuzzy Control)?", "id": "Kombinasi Jaringan Saraf, Logika Fuzzy." },
  { "en": "Apa Itu Sistem Hibrida (Hybrid System)?", "id": "Sistem Interaksi Dinamika Kontinu, Diskrit." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Robot." },
  { "en": "Apa Itu Konfigurasi (Configuration) Robot?", "id": "Posisi Semua Sambungan (Joint) Robot." },
  { "en": "Apa Itu Ruang Konfigurasi (Configuration Space)?", "id": "Ruang Semua Kemungkinan Konfigurasi." },
  { "en": "Apa Itu Perencanaan Gerak (Motion Planning)?", "id": "Mencari Jalur Gerak Bebas Hambatan." },
  { "en": "Algoritma Perencanaan Gerak (Motion Planning) Contoh?", "id": "A*, RRT (Rapidly-Exploring Random Tree)." },
  { "en": "Apa Itu Kontrol Impedansi (Impedance Control) Robot?", "id": "Mengontrol Hubungan Gaya, Posisi Robot." },
  { "en": "Apa Itu Kontrol Gaya (Force Control) Robot?", "id": "Mengontrol Gaya Interaksi Robot Lingkungan." },
  { "en": "Apa Itu Robot Kolaboratif (Collaborative Robot) (Cobot)?", "id": "Robot Aman Bekerja Samping Manusia." },
  { "en": "Apa Itu Robot Bergerak Otonom (Autonomous Mobile Robot)?", "id": "Robot Navigasi Mandiri Lingkungan." },
  { "en": "Apa Itu SLAM (Simultaneous Localization And Mapping)?", "id": "Robot Membangun Peta, Tentukan Lokasi." },
  { "en": "Sensor Apa Digunakan SLAM (Simultaneous Localization And Mapping)?", "id": "LiDAR, Kamera, IMU (Inertial Measurement Unit), Odometry." },
  { "en": "Apa Itu Kalman Filter (Kalman Filter) Dalam SLAM?", "id": "Menggabungkan Data Sensor Estimasi Posisi." },
  { "en": "Apa Itu Particle Filter (Filter Partikel) SLAM?", "id": "Metode Estimasi Probabilistik SLAM." },
  { "en": "Apa Itu Odometri (Odometry)?", "id": "Estimasi Posisi Berdasarkan Gerakan Roda." },
  { "en": "Apa Itu IMU (Inertial Measurement Unit)?", "id": "Unit Pengukuran Inersia." },
  { "en": "Sensor Apa Ada Di IMU (Inertial Measurement Unit)?", "id": "Akselerometer, Giroskop, Magnetometer." },
  { "en": "Apa Fungsi Akselerometer (Accelerometer)?", "id": "Mengukur Percepatan Linear." },
  { "en": "Apa Fungsi Giroskop (Gyroscope)?", "id": "Mengukur Kecepatan Sudut (Rotasi)." },
  { "en": "Apa Fungsi Magnetometer (Magnetometer)?", "id": "Mengukur Medan Magnet (Arah Utara)." },
  { "en": "Apa Itu Sensor Fusion (Fusi Sensor)?", "id": "Menggabungkan Data Berbagai Sensor." },
  { "en": "Mengapa Perlu Sensor Fusion (Fusi Sensor)?", "id": "Tingkatkan Akurasi, Keandalan Estimasi." },
  { "en": "Algoritma Fusi Sensor (Sensor Fusion) Contoh?", "id": "Filter Kalman, Complementary Filter." },
  { "en": "Apa Itu LiDAR (Light Detection And Ranging)?", "id": "Sensor Jarak Menggunakan Laser." },
  { "en": "Apa Prinsip Kerja LiDAR (Light Detection And Ranging)?", "id": "Mengukur Waktu Tempuh Pulsa Laser." },
  { "en": "Apa Itu Point Cloud (Awan Titik)?", "id": "Kumpulan Titik Data 3D Hasil LiDAR." },
  { "en": "Apa Itu Visi Komputer (Computer Vision)?", "id": "Kemampuan Komputer Interpretasi Gambar." },
  { "en": "Tugas Umum Visi Komputer (Computer Vision)?", "id": "Deteksi Objek, Segmentasi, Pengenalan Wajah." },
  { "en": "Apa Itu Deteksi Objek (Object Detection)?", "id": "Menemukan Lokasi, Jenis Objek Gambar." },
  { "en": "Apa Itu Segmentasi Gambar (Image Segmentation)?", "id": "Membagi Gambar Jadi Wilayah Bermakna." },
  { "en": "Apa Itu Convolutional Neural Network (CNN)?", "id": "Jaringan Saraf Tiruan Khusus Citra." },
  { "en": "Apa Kepanjangan CNN (Convolutional Neural Network)?", "id": "Jaringan Saraf Konvolusional." },
  { "en": "Apa Itu Lapisan Konvolusi (Convolution Layer)?", "id": "Lapisan Ekstraksi Fitur Citra CNN." },
  { "en": "Apa Itu Lapisan Pooling (Pooling Layer)?", "id": "Lapisan Pengurangan Dimensi Fitur CNN." },
  { "en": "Apa Itu Recurrent Neural Network (RNN)?", "id": "Jaringan Saraf Tiruan Data Sekuensial." },
  { "en": "Apa Kepanjangan RNN (Recurrent Neural Network)?", "id": "Jaringan Saraf Berulang." },
  { "en": "Apa Itu Long Short-Term Memory (LSTM)?", "id": "Jenis RNN (Recurrent Neural Network) Tangani Dependensi Jangka Panjang." },
  { "en": "Apa Kepanjangan LSTM (Long Short-Term Memory)?", "id": "Memori Jangka Pendek Panjang." },
  { "en": "Apa Itu Natural Language Processing (NLP)?", "id": "Pemrosesan Bahasa Alami Komputer." },
  { "en": "Apa Kepanjangan NLP (Natural Language Processing)?", "id": "Pemrosesan Bahasa Alami." },
  { "en": "Aplikasi NLP (Natural Language Processing)?", "id": "Terjemahan Mesin, Analisis Sentimen." },
  { "en": "Apa Itu Transformer (Model Arsitektur)?", "id": "Arsitektur Deep Learning Populer NLP." },
  { "en": "Apa Itu Reinforcement Learning (RL)?", "id": "Pembelajaran Berbasis Coba-Salah (Reward)." },
  { "en": "Apa Itu Agen (Agent) Dalam RL?", "id": "Entitas Yang Belajar Berinteraksi Lingkungan." },
  { "en": "Apa Itu Lingkungan (Environment) Dalam RL?", "id": "Dunia Tempat Agen Berinteraksi." },
  { "en": "Apa Itu Keadaan (State) Dalam RL?", "id": "Representasi Kondisi Lingkungan Saat Ini." },
  { "en": "Apa Itu Aksi (Action) Dalam RL?", "id": "Pilihan Tindakan Diambil Agen." },
  { "en": "Apa Itu Hadiah (Reward) Dalam RL?", "id": "Sinyal Umpan Balik Kinerja Agen." },
  { "en": "Apa Itu Kebijakan (Policy) Dalam RL?", "id": "Strategi Pemetaan Keadaan Ke Aksi." },
  { "en": "Apa Itu Algoritma Q-Learning (Q-Learning)?", "id": "Algoritma RL Belajar Nilai Aksi Keadaan." },
  { "en": "Apa Itu Deep Q-Network (DQN)?", "id": "Kombinasi Q-Learning Jaringan Saraf Dalam." },
  { "en": "Apa Itu Eksplorasi (Exploration) Vs Eksploitasi (Exploitation)?", "id": "Dilema Agen RL Coba Aksi Baru, Pilih Terbaik." },
  { "en": "Aplikasi Reinforcement Learning (RL)?", "id": "Game Playing (AlphaGo), Robotika, Kontrol." },
  { "en": "Apa Itu Sistem Rekomendasi (Recommender System)?", "id": "Sistem Prediksi Preferensi Pengguna." },
  { "en": "Jenis Sistem Rekomendasi (Recommender System)?", "id": "Content-Based, Collaborative Filtering." },
  { "en": "Apa Itu Content-Based Filtering (Penyaringan Berbasis Konten)?", "id": "Rekomendasi Berdasarkan Item Mirip Disukai." },
  { "en": "Apa Itu Collaborative Filtering (Penyaringan Kolaboratif)?", "id": "Rekomendasi Berdasarkan Pengguna Mirip." },
  { "en": "Apa Itu Cold Start Problem (Masalah Awal Dingin)?", "id": "Kesulitan Rekomendasi Pengguna/Item Baru." },
  { "en": "Apa Itu Metrik Evaluasi (Evaluation Metric) Rekomendasi?", "id": "Precision, Recall, F1-Score, MAP, NDCG." },
  { "en": "Apa Itu Machine Learning (Pembelajaran Mesin)?", "id": "Bidang AI (Artificial Intelligence) Belajar Dari Data." },
  { "en": "Apa Tiga Jenis Pembelajaran Mesin Utama?", "id": "Supervised, Unsupervised, Reinforcement." },
  { "en": "Apa Itu Supervised Learning (Pembelajaran Terawasi)?", "id": "Belajar Dari Data Berlabel (Input, Output)." },
  { "en": "Tugas Supervised Learning (Pembelajaran Terawasi) Umum?", "id": "Klasifikasi Dan Regresi." },
  { "en": "Apa Itu Klasifikasi (Classification)?", "id": "Memprediksi Kategori Diskrit." },
  { "en": "Apa Itu Regresi (Regression)?", "id": "Memprediksi Nilai Kontinu." },
  { "en": "Algoritma Klasifikasi (Classification) Contoh?", "id": "Logistic Regression, SVM, Decision Tree." },
  { "en": "Algoritma Regresi (Regression) Contoh?", "id": "Linear Regression, Polynomial Regression." },
  { "en": "Apa Itu Unsupervised Learning (Pembelajaran Tak Terawasi)?", "id": "Belajar Dari Data Tanpa Label." },
  { "en": "Tugas Unsupervised Learning (Pembelajaran Tak Terawasi) Umum?", "id": "Clustering, Pengurangan Dimensi." },
  { "en": "Apa Itu Clustering (Pengelompokan)?", "id": "Mengelompokkan Data Mirip Bersama." },
  { "en": "Algoritma Clustering (Pengelompokan) Contoh?", "id": "K-Means, DBSCAN, Hierarchical Clustering." },
  { "en": "Apa Itu Pengurangan Dimensi (Dimensionality Reduction)?", "id": "Mengurangi Jumlah Fitur Data." },
  { "en": "Algoritma Pengurangan Dimensi (Dimensionality Reduction) Contoh?", "id": "PCA (Principal Component Analysis), t-SNE (t-distributed Stochastic Neighbor Embedding)." },
  { "en": "Apa Itu Fitur (Feature) Dalam Machine Learning?", "id": "Variabel Input Yang Dapat Diukur." },
  { "en": "Apa Itu Label (Label) / Target?", "id": "Variabel Output Yang Diprediksi (Supervised)." },
  { "en": "Apa Itu Training Set (Set Pelatihan)?", "id": "Data Digunakan Latih Model." },
  { "en": "Apa Itu Test Set (Set Pengujian)?", "id": "Data Digunakan Evaluasi Model Terlatih." },
  { "en": "Apa Itu Validation Set (Set Validasi)?", "id": "Data Tuning Parameter Model (Hyperparameter)." },
  { "en": "Apa Itu Overfitting (Overfitting)?", "id": "Model Terlalu Cocok Data Latih, Buruk Data Baru." },
  { "en": "Apa Itu Underfitting (Underfitting)?", "id": "Model Terlalu Sederhana, Tidak Tangkap Pola." },
  { "en": "Bagaimana Mengatasi Overfitting (Overfitting)?", "id": "Regularisasi, Cross-Validation, Data Lebih Banyak." },
  { "en": "Apa Itu Regularisasi (Regularization)?", "id": "Teknik Menambah Penalti Kompleksitas Model." },
  { "en": "Apa Itu Cross-Validation (Validasi Silang)?", "id": "Teknik Evaluasi Model Bagi Data Lipatan." },
  { "en": "Apa Itu Hyperparameter (Hyperparameter)?", "id": "Parameter Diatur Sebelum Pelatihan Model." },
  { "en": "Apa Itu Feature Engineering (Rekayasa Fitur)?", "id": "Membuat Fitur Baru/Transformasi Data Mentah." },
  { "en": "Apa Itu Feature Scaling (Penskalaan Fitur)?", "id": "Mengubah Rentang Nilai Fitur (Normalisasi)." },
  { "en": "Metode Feature Scaling (Penskalaan Fitur) Umum?", "id": "Min-Max Scaling, Standardization (Z-score)." },
  { "en": "Apa Itu One-Hot Encoding (One-Hot Encoding)?", "id": "Representasi Fitur Kategorikal Jadi Biner." },
  { "en": "Apa Itu Matriks Konfusi (Confusion Matrix)?", "id": "Tabel Evaluasi Kinerja Klasifikasi." },
  { "en": "Metrik Apa Dari Matriks Konfusi?", "id": "Akurasi, Presisi, Recall, F1-Score." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Proporsi Prediksi Positif Yang Benar." },
  { "en": "Apa Itu Recall (Recall) / Sensitivitas?", "id": "Proporsi Positif Aktual Teridentifikasi Benar." },
  { "en": "Apa Itu F1-Score (F1-Score)?", "id": "Rata-Rata Harmonik Presisi, Recall." },
  { "en": "Apa Itu Kurva ROC (Receiver Operating Characteristic)?", "id": "Grafik True Positive Rate Vs False Positive Rate." },
  { "en": "Apa Itu AUC (Area Under Curve) ROC?", "id": "Ukuran Kinerja Klasifikasi Keseluruhan." },
  { "en": "Metrik Evaluasi (Evaluation Metric) Regresi Umum?", "id": "MAE, MSE, RMSE, R-squared." },
  { "en": "Apa Itu MAE (Mean Absolute Error)?", "id": "Rata-Rata Kesalahan Absolut." },
  { "en": "Apa Itu MSE (Mean Squared Error)?", "id": "Rata-Rata Kuadrat Kesalahan." },
  { "en": "Apa Itu RMSE (Root Mean Squared Error)?", "id": "Akar Kuadrat MSE (Satuan Sama Target)." },
  { "en": "Apa Itu R-squared (Koefisien Determinasi)?", "id": "Proporsi Varians Target Dijelaskan Model." },
  { "en": "Apa Itu Ensemble Learning (Pembelajaran Ensemble)?", "id": "Menggabungkan Beberapa Model Jadi Satu." },
  { "en": "Contoh Metode Ensemble (Ensemble Method)?", "id": "Bagging (Random Forest), Boosting (AdaBoost, XGBoost)." },
  { "en": "Apa Itu Random Forest (Hutan Acak)?", "id": "Ensemble Pohon Keputusan (Decision Tree) (Bagging)." },
  { "en": "Apa Itu Boosting (Boosting)?", "id": "Ensemble Model Belajar Urutan Perbaiki Error." },
  { "en": "Apa Itu Support Vector Machine (SVM)?", "id": "Algoritma Klasifikasi Cari Hyperplane Optimal." },
  { "en": "Apa Itu Kernel Trick (Trik Kernel) SVM?", "id": "Memetakan Data Ruang Dimensi Tinggi." },
  { "en": "Apa Itu K-Nearest Neighbors (KNN)?", "id": "Algoritma Klasifikasi Berbasis Tetangga Terdekat." },
  { "en": "Apa Itu Pohon Keputusan (Decision Tree)?", "id": "Model Klasifikasi/Regresi Struktur Pohon." },
  { "en": "Apa Itu Deep Learning (Pembelajaran Mendalam)?", "id": "Sub-Bidang ML (Machine Learning) Jaringan Saraf Dalam." },
  { "en": "Apa Itu Aktivasi Fungsi (Activation Function)?", "id": "Fungsi Non-Linear Output Neuron." },
  { "en": "Contoh Aktivasi Fungsi (Activation Function)?", "id": "ReLU, Sigmoid, Tanh." },
  { "en": "Apa Itu Backpropagation (Propagasi Balik)?", "id": "Algoritma Latih Jaringan Saraf Tiruan." },
  { "en": "Apa Itu Gradient Descent (Penurunan Gradien)?", "id": "Algoritma Optimasi Cari Minimum Fungsi." },
  { "en": "Apa Itu Learning Rate (Laju Pembelajaran)?", "id": "Ukuran Langkah Penyesuaian Bobot Jaringan." },
  { "en": "Apa Itu Optimasi Momentum (Momentum Optimization)?", "id": "Variasi Gradient Descent Percepat Konvergensi." },
  { "en": "Apa Itu Optimasi Adam (Adam Optimizer)?", "id": "Algoritma Optimasi Populer Deep Learning." },
  { "en": "Apa Itu Regularisasi L1 (L1 Regularization) (Lasso)?", "id": "Menambah Penalti Jumlah Absolut Bobot." },
  { "en": "Apa Efek Regularisasi L1 (L1 Regularization)?", "id": "Mendorong Bobot Menuju Nol (Seleksi Fitur)." },
  { "en": "Apa Itu Regularisasi L2 (L2 Regularization) (Ridge)?", "id": "Menambah Penalti Jumlah Kuadrat Bobot." },
  { "en": "Apa Efek Regularisasi L2 (L2 Regularization)?", "id": "Mengecilkan Bobot (Mencegah Overfitting)." },
  { "en": "Apa Itu Dropout (Dropout) Regularisasi?", "id": "Menonaktifkan Neuron Acak Saat Pelatihan." },
  { "en": "Apa Itu Batch Normalization (Normalisasi Batch)?", "id": "Menormalisasi Input Lapisan Jaringan Saraf." },
  { "en": "Manfaat Batch Normalization (Normalisasi Batch)?", "id": "Mempercepat Pelatihan, Meningkatkan Stabilitas." },
  { "en": "Apa Itu Transfer Learning (Pembelajaran Transfer)?", "id": "Menggunakan Model Terlatih Tugas Terkait." },
  { "en": "Apa Itu Fine-Tuning (Penyesuaian Halus)?", "id": "Melatih Ulang Sebagian Model Transfer Learning." },
  { "en": "Apa Itu Convolutional Neural Network (CNN)?", "id": "Jaringan Saraf Khusus Data Grid (Gambar)." },
  { "en": "Apa Itu Operasi Konvolusi (Convolution Operation)?", "id": "Filter (Kernel) Geser Atas Input." },
  { "en": "Apa Itu Filter (Filter) / Kernel (Kernel) CNN?", "id": "Matriks Kecil Pendeteksi Pola (Tepi)." },
  { "en": "Apa Itu Peta Fitur (Feature Map)?", "id": "Output Lapisan Konvolusi (Representasi Fitur)." },
  { "en": "Apa Itu Lapisan Pooling (Pooling Layer)?", "id": "Mengurangi Dimensi Peta Fitur (Downsampling)." },
  { "en": "Jenis Pooling (Pooling) Umum?", "id": "Max Pooling, Average Pooling." },
  { "en": "Apa Itu Max Pooling (Max Pooling)?", "id": "Mengambil Nilai Maksimum Area Lokal." },
  { "en": "Apa Itu Lapisan Fully Connected (Terhubung Penuh)?", "id": "Lapisan Akhir CNN Klasifikasi/Regresi." },
  { "en": "Arsitektur CNN (Convolutional Neural Network) Terkenal?", "id": "LeNet, AlexNet, VGG, ResNet, Inception." },
  { "en": "Apa Itu Recurrent Neural Network (RNN)?", "id": "Jaringan Saraf Data Sekuensial (Memori)." },
  { "en": "Masalah RNN (Recurrent Neural Network) Dasar?", "id": "Vanishing/Exploding Gradient." },
  { "en": "Apa Itu Vanishing Gradient (Gradien Menghilang)?", "id": "Gradien Jadi Sangat Kecil Sulit Belajar." },
  { "en": "Apa Itu Exploding Gradient (Gradien Meledak)?", "id": "Gradien Jadi Sangat Besar (Instabilitas)." },
  { "en": "Apa Itu Long Short-Term Memory (LSTM)?", "id": "Jenis RNN (Recurrent Neural Network) Atasi Vanishing Gradient." },
  { "en": "Apa Itu Gated Recurrent Unit (GRU)?", "id": "Jenis RNN (Recurrent Neural Network) Lebih Sederhana LSTM." },
  { "en": "Apa Kepanjangan GRU (Gated Recurrent Unit)?", "id": "Unit Berulang Bergerbang." },
  { "en": "Aplikasi RNN (Recurrent Neural Network)/LSTM (Long Short-Term Memory)/GRU (Gated Recurrent Unit)?", "id": "NLP, Prediksi Runtun Waktu, Pengenalan Suara." },
  { "en": "Apa Itu Model Sequence-to-Sequence (Seq2Seq)?", "id": "Model Input Sekuens, Output Sekuens." },
  { "en": "Aplikasi Model Seq2Seq (Sequence-to-Sequence)?", "id": "Terjemahan Mesin, Ringkasan Teks." },
  { "en": "Apa Itu Mekanisme Perhatian (Attention Mechanism)?", "id": "Mekanisme Fokus Bagian Input Relevan." },
  { "en": "Apa Itu Model Transformer (Transformer)?", "id": "Arsitektur Berbasis Mekanisme Perhatian." },
  { "en": "Keunggulan Model Transformer (Transformer)?", "id": "Paralelisasi Tinggi, Tangani Dependensi Jauh." },
  { "en": "Model Bahasa (Language Model) Besar Contoh?", "id": "GPT (Generative Pre-trained Transformer), BERT (Bidirectional Encoder Representations from Transformers)." },
  { "en": "Apa Kepanjangan GPT (Generative Pre-trained Transformer)?", "id": "Transformer Generatif Pra-Terlatih." },
  { "en": "Apa Kepanjangan BERT (Bidirectional Encoder Representations from Transformers)?", "id": "Representasi Encoder Dua Arah Dari Transformer." },
  { "en": "Apa Itu Word Embedding (Penyematan Kata)?", "id": "Representasi Vektor Kata Bermakna Semantik." },
  { "en": "Contoh Algoritma Word Embedding (Penyematan Kata)?", "id": "Word2Vec, GloVe." },
  { "en": "Apa Itu Generative Adversarial Network (GAN)?", "id": "Model Generatif Dua Jaringan (Generator, Diskriminator)." },
  { "en": "Aplikasi GAN (Generative Adversarial Network)?", "id": "Generasi Gambar Realistis, Super-Resolusi." },
  { "en": "Apa Itu Autoencoder (Autoencoder)?", "id": "Jaringan Saraf Tak Terawasi (Kompresi Data)." },
  { "en": "Apa Struktur Autoencoder (Autoencoder)?", "id": "Encoder (Encoder) Dan Decoder (Decoder)." },
  { "en": "Apa Fungsi Encoder (Encoder) Autoencoder?", "id": "Mengkompresi Input Ke Representasi Laten." },
  { "en": "Apa Fungsi Decoder (Decoder) Autoencoder?", "id": "Merekonstruksi Input Dari Representasi Laten." },
  { "en": "Aplikasi Autoencoder (Autoencoder)?", "id": "Reduksi Dimensi, Deteksi Anomali, Denoising." },
  { "en": "Apa Itu Variational Autoencoder (VAE)?", "id": "Jenis Autoencoder Generatif Probabilistik." },
  { "en": "Apa Kepanjangan VAE (Variational Autoencoder)?", "id": "Autoencoder Variasional." },
  { "en": "Apa Itu Reinforcement Learning (RL)?", "id": "Agen Belajar Aksi Optimal Interaksi Lingkungan." },
  { "en": "Apa Itu Fungsi Nilai (Value Function) RL?", "id": "Estimasi Reward Masa Depan Keadaan Tertentu." },
  { "en": "Apa Itu Fungsi Q (Q-Function) RL?", "id": "Estimasi Reward Masa Depan Aksi Tertentu Keadaan." },
  { "en": "Apa Itu Policy Gradient (Gradien Kebijakan) RL?", "id": "Metode RL Langsung Optimasi Kebijakan." },
  { "en": "Apa Itu Actor-Critic (Aktor-Kritik) RL?", "id": "Metode RL Kombinasi Policy Gradient, Value Function." },
  { "en": "Apa Itu Meta-Learning (Pembelajaran Meta)?", "id": "Belajar Cara Belajar (Adaptasi Cepat)." },
  { "en": "Apa Itu Federated Learning (Pembelajaran Federasi)?", "id": "Melatih Model Terdistribusi Data Lokal." },
  { "en": "Manfaat Federated Learning (Pembelajaran Federasi)?", "id": "Menjaga Privasi Data Pengguna." },
  { "en": "Apa Itu Explainable AI (XAI)?", "id": "Membuat Keputusan Model AI Dapat Dimengerti." },
  { "en": "Mengapa XAI (Explainable Artificial Intelligence) Penting?", "id": "Transparansi, Kepercayaan, Debugging Model." },
  { "en": "Metode XAI (Explainable Artificial Intelligence) Lokal Contoh?", "id": "LIME (Local Interpretable Model-agnostic Explanations), SHAP (SHapley Additive exPlanations)." },
  { "en": "Apa Kepanjangan LIME (Local Interpretable Model-agnostic Explanations)?", "id": "Penjelasan Agnostik Model Dapat Diinterpretasi Lokal." },
  { "en": "Apa Kepanjangan SHAP (SHapley Additive exPlanations)?", "id": "Penjelasan Aditif Shapley." },
  { "en": "Apa Itu Bias (Bias) Algoritma?", "id": "Hasil Sistematis Tidak Adil Algoritma ML." },
  { "en": "Penyebab Bias (Bias) Algoritma?", "id": "Data Latih Bias, Desain Algoritma." },
  { "en": "Apa Itu Fairness (Keadilan) Dalam AI?", "id": "Memastikan Model AI Tidak Diskriminatif." },
  { "en": "Apa Itu Robustness (Kekuatan) AI?", "id": "Kemampuan Model Tahan Gangguan Input." },
  { "en": "Apa Itu Serangan Adversarial (Adversarial Attack)?", "id": "Input Dirancang Menipu Model ML." },
  { "en": "Apa Itu Pertahanan Adversarial (Adversarial Defense)?", "id": "Teknik Membuat Model Tahan Serangan." },
  { "en": "Apa Itu Privasi Diferensial (Differential Privacy)?", "id": "Teknik Menambah Noise Jaga Privasi Data." },
  { "en": "Apa Itu Etika AI (AI Ethics)?", "id": "Prinsip Moral Pengembangan, Penggunaan AI." },
  { "en": "Apa Itu Komputasi Awan (Cloud Computing)?", "id": "Penyediaan Sumber Daya Komputasi Internet." },
  { "en": "Model Penyebaran Awan (Cloud Deployment Model)?", "id": "Publik, Privat, Hibrida, Multi-Awan." },
  { "en": "Apa Itu Awan Publik (Public Cloud)?", "id": "Sumber Daya Dimiliki Penyedia Layanan." },
  { "en": "Apa Itu Awan Privat (Private Cloud)?", "id": "Sumber Daya Didedikasikan Satu Organisasi." },
  { "en": "Apa Itu Awan Hibrida (Hybrid Cloud)?", "id": "Kombinasi Awan Publik Dan Privat." },
  { "en": "Apa Itu Multi-Awan (Multi-Cloud)?", "id": "Menggunakan Layanan Lebih Satu Penyedia Awan." },
  { "en": "Penyedia Awan (Cloud Provider) Utama?", "id": "AWS, Azure, GCP." },
  { "en": "Apa Itu AWS (Amazon Web Services)?", "id": "Platform Awan Amazon." },
  { "en": "Apa Itu Microsoft Azure (Azure)?", "id": "Platform Awan Microsoft." },
  { "en": "Apa Itu Google Cloud Platform (GCP)?", "id": "Platform Awan Google." },
  { "en": "Apa Itu Wilayah (Region) Awan?", "id": "Lokasi Geografis Pusat Data Awan." },
  { "en": "Apa Itu Zona Ketersediaan (Availability Zone) (AZ)?", "id": "Pusat Data Terisolasi Dalam Satu Wilayah." },
  { "en": "Mengapa Gunakan Multi-AZ (Multi-Availability Zone)?", "id": "Ketersediaan Tinggi (High Availability)." },
  { "en": "Apa Itu Komputasi Elastis (Elastic Compute)?", "id": "Kemampuan Menambah/Mengurangi Sumber Daya Komputasi." },
  { "en": "Contoh Layanan Komputasi Awan?", "id": "AWS EC2, Azure VMs, Google Compute Engine." },
  { "en": "Apa Itu Penyimpanan Objek (Object Storage)?", "id": "Penyimpanan Skalabel Data Tak Terstruktur." },
  { "en": "Contoh Layanan Penyimpanan Objek?", "id": "AWS S3, Azure Blob Storage, Google Cloud Storage." },
  { "en": "Apa Itu Penyimpanan Blok (Block Storage)?", "id": "Penyimpanan Mirip Hard Drive Virtual." },
  { "en": "Contoh Layanan Penyimpanan Blok?", "id": "AWS EBS, Azure Managed Disks." },
  { "en": "Apa Itu Penyimpanan Berkas (File Storage)?", "id": "Penyimpanan Berkas Terdistribusi (NAS di Awan)." },
  { "en": "Contoh Layanan Penyimpanan Berkas?", "id": "AWS EFS, Azure Files." },
  { "en": "Apa Itu Basis Data Sebagai Layanan (DBaaS)?", "id": "Layanan Basis Data Dikelola Awan." },
  { "en": "Apa Kepanjangan DBaaS (Database as a Service)?", "id": "Basis Data Sebagai Layanan." },
  { "en": "Contoh Layanan DBaaS (Database as a Service) Relasional?", "id": "AWS RDS, Azure SQL Database." },
  { "en": "Apa Itu Jaringan Awan Virtual (Virtual Private Cloud)?", "id": "Jaringan Privat Terisolasi Di Awan Publik." },
  { "en": "Apa Kepanjangan VPC (Virtual Private Cloud)?", "id": "Awan Pribadi Virtual." },
  { "en": "Apa Itu Kelompok Keamanan (Security Group)?", "id": "Firewall Virtual Atur Lalu Lintas VM." },
  { "en": "Apa Itu Load Balancer (Penyeimbang Beban) Awan?", "id": "Distribusi Lalu Lintas Aplikasi Awan." },
  { "en": "Apa Itu Auto Scaling (Penskalaan Otomatis)?", "id": "Menyesuaikan Jumlah Sumber Daya Otomatis." },
  { "en": "Apa Itu Content Delivery Network (CDN) Awan?", "id": "Distribusi Konten Cache Global." },
  { "en": "Apa Itu DNS (Domain Name System) Awan?", "id": "Layanan DNS (Domain Name System) Dikelola Awan." },
  { "en": "Apa Itu Komputasi Serverless (Serverless Computing)?", "id": "Eksekusi Kode Tanpa Kelola Server." },
  { "en": "Apa Itu Function as a Service (FaaS)?", "id": "Model Eksekusi Kode Serverless." },
  { "en": "Apa Itu Kontainer (Container) Sebagai Layanan (CaaS)?", "id": "Layanan Menjalankan, Mengelola Kontainer." },
  { "en": "Apa Kepanjangan CaaS (Containers as a Service)?", "id": "Kontainer Sebagai Layanan." },
  { "en": "Contoh Layanan CaaS (Containers as a Service)?", "id": "AWS ECS/EKS, Azure Kubernetes Service (AKS)." },
  { "en": "Apa Itu Platform Sebagai Layanan (PaaS)?", "id": "Platform Pengembangan, Deployment Aplikasi." },
  { "en": "Contoh Layanan PaaS (Platform as a Service)?", "id": "Heroku, Google App Engine." },
  { "en": "Apa Itu Perangkat Lunak Sebagai Layanan (SaaS)?", "id": "Aplikasi Disediakan Lewat Internet." },
  { "en": "Contoh Layanan SaaS (Software as a Service)?", "id": "Gmail, Salesforce, Microsoft 365." },
  { "en": "Apa Itu Infrastruktur Sebagai Kode (IaC)?", "id": "Mengelola Infrastruktur Melalui Kode." },
  { "en": "Manfaat IaC (Infrastructure as Code)?", "id": "Otomatisasi, Konsistensi, Version Control." },
  { "en": "Alat IaC (Infrastructure as Code) Populer?", "id": "Terraform, Ansible, CloudFormation." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Integrasi Kode Otomatis Secara Berkala." },
  { "en": "Apa Itu Continuous Delivery/Deployment (CD)?", "id": "Penyebaran Kode Otomatis Ke Lingkungan." },
  { "en": "Apa Itu Pipeline (Pipeline) CI/CD?", "id": "Alur Kerja Otomatis Build, Test, Deploy." },
  { "en": "Apa Itu Pengujian Otomatis (Automated Testing)?", "id": "Menjalankan Tes Menggunakan Skrip Otomatis." },
  { "en": "Jenis Pengujian Otomatis (Automated Testing)?", "id": "Unit Test, Integration Test, End-to-End." },
  { "en": "Apa Itu Pemantauan (Monitoring)?", "id": "Mengamati Kinerja Dan Kesehatan Sistem." },
  { "en": "Apa Itu Pencatatan Log (Logging)?", "id": "Merekam Kejadian (Event) Dalam Sistem." },
  { "en": "Apa Itu Metrik (Metrics)?", "id": "Pengukuran Kuantitatif Kinerja Sistem." },
  { "en": "Apa Itu Peringatan (Alerting)?", "id": "Notifikasi Otomatis Kondisi Abnormal." },
  { "en": "Apa Itu Devops (Development and Operations)?", "id": "Budaya Kolaborasi Pengembangan, Operasi." },
  { "en": "Tujuan Devops (Development and Operations)?", "id": "Mempercepat Pengiriman Software Berkualitas." },
  { "en": "Apa Itu Site Reliability Engineering (SRE)?", "id": "Pendekatan Rekayasa Keandalan Operasi." },
  { "en": "Konsep Kunci SRE (Site Reliability Engineering)?", "id": "SLO, SLI, Error Budget." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Perlindungan Sistem Komputer Dari Ancaman." },
  { "en": "Apa Tiga Pilar Keamanan Informasi (CIA Triad)?", "id": "Confidentiality, Integrity, Availability." },
  { "en": "Apa Kepanjangan CIA (Confidentiality Integrity Availability)?", "id": "Kerahasiaan, Integritas, Ketersediaan." },
  { "en": "Apa Itu Kerahasiaan (Confidentiality)?", "id": "Mencegah Akses Data Tidak Sah." },
  { "en": "Apa Itu Integritas (Integrity)?", "id": "Menjaga Keakuratan, Konsistensi Data." },
  { "en": "Apa Itu Ketersediaan (Availability)?", "id": "Memastikan Sistem Dapat Diakses Pengguna Sah." },
  { "en": "Apa Itu Malware (Malicious Software)?", "id": "Perangkat Lunak Jahat Merusak Sistem." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Mengubah Data Jadi Tidak Terbaca." },
  { "en": "Apa Itu Kunci Enkripsi (Encryption Key)?", "id": "Informasi Rahasia Proses Enkripsi/Dekripsi." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Mengontrol Lalu Lintas Jaringan Masuk/Keluar." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Membuat Terowongan Aman Jaringan Publik." },
  { "en": "Apa Itu Otentikasi (Authentication)?", "id": "Verifikasi Identitas Pengguna Atau Sistem." },
  { "en": "Apa Itu Otorisasi (Authorization)?", "id": "Pemberian Hak Akses Sumber Daya." },
  { "en": "Apa Itu Elektronika Dasar (Basic Electronics)?", "id": "Prinsip Fundamental Komponen, Rangkaian Listrik." },
  { "en": "Apa Itu Arus Listrik (Electric Current)?", "id": "Aliran Muatan Listrik (Elektron)." },
  { "en": "Satuan Arus Listrik (Electric Current)?", "id": "Ampere (A)." },
  { "en": "Apa Itu Tegangan Listrik (Voltage)?", "id": "Beda Potensial Penggerak Arus." },
  { "en": "Satuan Tegangan Listrik (Voltage)?", "id": "Volt (V)." },
  { "en": "Apa Itu Resistansi Listrik (Resistance)?", "id": "Hambatan Terhadap Aliran Arus." },
  { "en": "Satuan Resistansi Listrik (Resistance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Hukum Ohm (Ohm's Law)?", "id": "V = I * R." },
  { "en": "Apa Itu Daya Listrik (Power)?", "id": "Laju Penggunaan Energi Listrik." },
  { "en": "Satuan Daya Listrik (Power)?", "id": "Watt (W)." },
  { "en": "Rumus Daya Listrik (Power)?", "id": "P = V * I." },
  { "en": "Apa Itu Rangkaian Seri (Series Circuit)?", "id": "Komponen Terhubung Satu Jalur." },
  { "en": "Arus Dalam Rangkaian Seri (Series Circuit)?", "id": "Sama Di Semua Komponen." },
  { "en": "Resistansi Total (Total Resistance) Seri?", "id": "Jumlah Semua Resistansi." },
  { "en": "Apa Itu Rangkaian Paralel (Parallel Circuit)?", "id": "Komponen Terhubung Banyak Jalur." },
  { "en": "Tegangan Dalam Rangkaian Paralel (Parallel Circuit)?", "id": "Sama Di Semua Komponen." },
  { "en": "Resistansi Total (Total Resistance) Paralel?", "id": "Lebih Kecil Dari Resistansi Terkecil." },
  { "en": "Apa Itu Hukum Kirchhoff (Kirchhoff's Laws)?", "id": "Hukum Arus (KCL) Tegangan (KVL)." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Satuan Kapasitansi (Capacitance)?", "id": "Farad (F)." },
  { "en": "Fungsi Kapasitor (Capacitor) Dalam DC?", "id": "Memblokir Arus Setelah Terisi Penuh." },
  { "en": "Apa Itu Induktor (Inductor)?", "id": "Komponen Penyimpan Energi Medan Magnet." },
  { "en": "Satuan Induktansi (Inductance)?", "id": "Henry (H)." },
  { "en": "Fungsi Induktor (Inductor) Dalam DC?", "id": "Menjadi Short Circuit Setelah Tunak." },
  { "en": "Apa Itu Arus Bolak-Balik (AC)?", "id": "Arus Berubah Arah Secara Periodik." },
  { "en": "Apa Itu Frekuensi (Frequency) AC?", "id": "Jumlah Siklus Per Detik (Hz)." },
  { "en": "Apa Itu Impedansi (Impedance) (Z)?", "id": "Hambatan Total Rangkaian AC." },
  { "en": "Apa Itu Reaktansi (Reactance) (X)?", "id": "Hambatan Kapasitor/Induktor Terhadap AC." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Ukuran Efisiensi Penggunaan Daya AC." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Penyearah Arus." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Mengubah Sinyal AC Menjadi DC." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Semikonduktor Penguat/Saklar." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Meningkatkan Kekuatan Sinyal Elektronik." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Menghasilkan Sinyal Periodik Elektronik." },
  { "en": "Apa Itu Filter (Filter) Elektronik?", "id": "Memilih Atau Menolak Frekuensi Tertentu." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Apa Itu Logika Biner (Binary Logic)?", "id": "Sistem Logika Dua Nilai (0, 1)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Digital Dasar." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Ke Gerakan." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Mendapatkan Nilai Besaran." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Sinyal Listrik." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor Listrik (Electric Motor)?", "id": "Mengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator Listrik (Electric Generator)?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Bumi Untuk Keselamatan." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Berlebih (Putus)." },
  { "en": "Apa Itu Circuit Breaker (Pemutus Sirkuit)?", "id": "Pelindung Arus Berlebih (Dapat Direset)." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Jaringan Listrik Skala Besar." },
  { "en": "Apa Itu Transmisi (Transmission) Daya?", "id": "Penyaluran Daya Jarak Jauh." },
  { "en": "Apa Itu Distribusi (Distribution) Daya?", "id": "Penyaluran Daya Ke Pengguna Akhir." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Pengubah Tegangan Jaringan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Aliran Daya Semikonduktor." },
  { "en": "Apa Itu Penyearah Terkendali (Controlled Rectifier)?", "id": "Penyearah Menggunakan Thyristor (SCR)." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah Daya DC Menjadi AC." },
  { "en": "Apa Itu Konverter DC-DC (DC-DC Converter)?", "id": "Mengubah Level Tegangan DC." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa Kontrol." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Loop Tertutup (Closed Loop)?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Kontroler PID (PID Controller)?", "id": "Kontroler Proporsional-Integral-Derivatif." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Logika Industri." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Pertukaran Data Antar Perangkat." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputasi." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Global Antar Jaringan." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Data." },
  { "en": "Apa Itu IP Address (Alamat IP)?", "id": "Alamat Numerik Perangkat Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Jaringan Berbeda." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Interaksi Medan Listrik, Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Radiasi/Penerima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Proses Penumpangan Sinyal Informasi." },
  { "en": "Apa Itu Frekuensi Radio (Radio Frequency) (RF)?", "id": "Frekuensi Gelombang EM Komunikasi Radio." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Mendapatkan Nilai Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Seberapa Dekat Ukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Seberapa Konsisten Pengukuran Berulang." },
  { "en": "Apa Itu Besaran (Quantity) Listrik Dasar?", "id": "Tegangan, Arus, Resistansi, Daya." },
  { "en": "Apa Itu Unit (Unit) SI?", "id": "Sistem Satuan Internasional." },
  { "en": "Apa Itu Prefiks (Prefix) SI?", "id": "Awalan Satuan (Kilo, Mega, Mili)." },
  { "en": "Contoh Prefiks (Prefix) SI Besar?", "id": "Kilo (k), Mega (M), Giga (G)." },
  { "en": "Contoh Prefiks (Prefix) SI Kecil?", "id": "Mili (m), Mikro (µ), Nano (n)." },
  { "en": "Apa Fungsi Kapasitor Kopling (Coupling Capacitor)?", "id": "Melewatkan AC, Memblokir Komponen DC." },
  { "en": "Apa Fungsi Kapasitor Bypass (Bypass Capacitor)?", "id": "Melewatkan Noise AC Ke Ground." },
  { "en": "Apa Fungsi Kapasitor Decoupling (Decoupling Capacitor)?", "id": "Menstabilkan Tegangan Catu Daya Lokal." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RC?", "id": "Ukuran Waktu Pengisian/Pengosongan Kapasitor." },
  { "en": "Apa Rumus Konstanta Waktu (Time Constant) RC?", "id": "Tau (τ) Sama Dengan R Kali C." },
  { "en": "Apa Itu Konstanta Waktu (Time Constant) RL?", "id": "Ukuran Waktu Arus Induktor Berubah." },
  { "en": "Apa Rumus Konstanta Waktu (Time Constant) RL?", "id": "Tau (τ) Sama Dengan L/R." },
  { "en": "Bagaimana Perilaku Kapasitor (Capacitor) Terhadap DC?", "id": "Menjadi Rangkaian Terbuka Saat Tunak." },
  { "en": "Bagaimana Perilaku Induktor (Inductor) Terhadap DC?", "id": "Menjadi Hubungan Singkat Saat Tunak." },
  { "en": "Apa Itu Impedansi (Impedance) Kompleks?", "id": "Representasi Fasor Resistansi Dan Reaktansi." },
  { "en": "Apa Itu Admitansi (Admittance)?", "id": "Kebalikan Dari Impedansi (Y = 1/Z)." },
  { "en": "Apa Satuan Admitansi (Admittance)?", "id": "Siemens (S)." },
  { "en": "Apa Itu Resonansi (Resonance) Seri?", "id": "Kondisi Impedansi Minimum Rangkaian RLC." },
  { "en": "Apa Itu Resonansi (Resonance) Paralel?", "id": "Kondisi Impedansi Maksimum Rangkaian RLC." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Ukuran Selektivitas Rangkaian Resonansi." },
  { "en": "Q (Faktor Kualitas) Tinggi Menandakan Bandwidth Bagaimana?", "id": "Bandwidth (Lebar Pita) Menjadi Sempit." },
  { "en": "Apa Itu Filter Pasif (Passive Filter)?", "id": "Filter Dibuat Dari Komponen Pasif." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Batas Operasi Filter (-3dB)." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman Slope Filter." },
  { "en": "Apa Itu Dioda Penyearah (Rectifier Diode)?", "id": "Dioda Mengubah AC Menjadi DC." },
  { "en": "Apa Itu Tegangan Maju (Forward Voltage) (Vf)?", "id": "Jatuh Tegangan Dioda Saat Konduksi." },
  { "en": "Apa Itu Arus Bocor Balik (Reverse Leakage)?", "id": "Arus Kecil Saat Bias Mundur." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Dioda Regulator Tegangan Mundur." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Pemancar Cahaya Elektroluminesensi." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Intensitas Cahaya." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Komponen Tiga Terminal Terkontrol Arus." },
  { "en": "Apa Tiga Daerah Operasi BJT?", "id": "Cutoff, Aktif, Dan Saturasi." },
  { "en": "Apa Itu Penguatan Arus (Beta/hFE) BJT?", "id": "Rasio Arus Kolektor Ke Basis." },
  { "en": "Apa Itu Transistor FET (Field Effect Transistor)?", "id": "Komponen Tiga Terminal Terkontrol Tegangan." },
  { "en": "Apa Keunggulan FET Dibanding BJT?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Jenis FET Gerbang Terisolasi Oksida." },
  { "en": "Apa Itu Tegangan Threshold (Vt) MOSFET?", "id": "Tegangan Gate Minimum Aktifkan Saluran." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Virtual Ground (Tanah Virtual) Op-Amp?", "id": "Input Inverting Dianggap Nol Volt." },
  { "en": "Apa Fungsi Umpan Balik Negatif Op-Amp?", "id": "Mengatur Penguatan Menjadi Stabil." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Op-Amp Dengan Gain Satu." },
  { "en": "Apa Fungsi Pengikut Tegangan (Voltage Follower)?", "id": "Sebagai Penyangga (Buffer) Impedansi." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio) Op-Amp?", "id": "Kemampuan Menolak Sinyal Mode Sama." },
  { "en": "Apa Itu Slew Rate (Slew Rate) Op-Amp?", "id": "Kecepatan Maksimum Perubahan Tegangan Output." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Dasar Rangkaian Logika Digital." },
  { "en": "Gerbang Apa Yang Outputnya Inversi AND?", "id": "Gerbang Logika NAND." },
  { "en": "Gerbang Apa Yang Outputnya Inversi OR?", "id": "Gerbang Logika NOR." },
  { "en": "Gerbang Apa Outputnya 1 Jika Input Ganjil?", "id": "Gerbang Logika XOR." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Word Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Menghitung Jumlah Pulsa Clock." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Memilih Satu Input Dari Banyak." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Mengarahkan Satu Input Ke Banyak Output." },
  { "en": "Apa Itu Memori RAM (Random Access Memory)?", "id": "Memori Baca Tulis Akses Acak (Volatil)." },
  { "en": "Apa Itu Memori ROM (Read Only Memory)?", "id": "Memori Hanya Baca (Non-Volatil)." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Besaran Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Besaran Digital Ke Analog." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip IC." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Ke Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Ke Gerakan Fisik." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Komponen Pasif Mengubah Tegangan AC." },
  { "en": "Apa Prinsip Dasar Transformator (Transformer)?", "id": "Induksi Elektromagnetik Mutual." },
  { "en": "Apa Itu Motor Induksi (Induction Motor)?", "id": "Motor AC Asinkron Umum Digunakan." },
  { "en": "Apa Itu Slip (Slip) Motor Induksi?", "id": "Perbedaan Kecepatan Medan Putar Rotor." },
  { "en": "Apa Itu Generator Sinkron (Synchronous Generator)?", "id": "Pembangkit Utama Listrik AC (Alternator)." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Jaringan Listrik Pembangkit Hingga Beban." },
  { "en": "Apa Itu Transmisi (Transmission) Listrik?", "id": "Penyaluran Daya Besar Tegangan Tinggi." },
  { "en": "Apa Itu Distribusi (Distribution) Listrik?", "id": "Penyaluran Daya Ke Konsumen Akhir." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Pengubah Tegangan, Switching." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Dari Kondisi Gangguan." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Pelindung Otomatis Arus Lebih." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Pendeteksi Gangguan Memberi Sinyal Trip." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Bumi Jaga Keamanan." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Aplikasi Semikonduktor Kontrol Daya." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Konverter AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Konverter DC Ke AC." },
  { "en": "Apa Itu Konverter DC-DC (DC-DC Converter)?", "id": "Pengubah Level Tegangan Searah (DC)." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Saklar Elektronik Daya." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Penggunaan Sinyal Output Kontrol Input." },
  { "en": "Apa Itu Kontroler PID (PID Controller)?", "id": "Kontroler Proporsional, Integral, Derivatif." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Digital Otomatisasi Proses Industri." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Transfer Informasi Bentuk Digital." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Standar Komunikasi Jaringan." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Global Jaringan Komputer." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Penentuan Nilai Kuantitatif Besaran." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Hasil Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Tingkat Keterulangan Hasil Pengukuran." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Dasar Terpadu." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang Listrik." },
  { "en": "Apa Itu Medan Elektromagnetik (Electromagnetic Field)?", "id": "Kombinasi Medan Listrik, Medan Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Konversi Sinyal Listrik, Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Proses Menumpangkan Sinyal Informasi Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Proses Ekstraksi Sinyal Informasi Pembawa." },
  { "en": "Apa Itu Listrik Statis (Static Electricity)?", "id": "Akumulasi Muatan Listrik Permukaan." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Aliran Listrik Statis Tiba-Tiba." },
  { "en": "Apa Itu Keselamatan Listrik (Electrical Safety)?", "id": "Praktik Pencegahan Kecelakaan Listrik." },
  { "en": "Apa Itu Bahan Konduktif (Conductive Material)?", "id": "Bahan Mudah Alirkan Arus Listrik." },
  { "en": "Apa Itu Bahan Isolator (Insulating Material)?", "id": "Bahan Sulit Alirkan Arus Listrik." },
  { "en": "Apa Itu Bahan Semikonduktor (Semiconductor Material)?", "id": "Bahan Sifat Antara Konduktor, Isolator." },
  { "en": "Apa Itu Sirkuit Terpadu Analog (Analog IC)?", "id": "IC (Integrated Circuit) Proses Sinyal Analog." },
  { "en": "Contoh Sirkuit Terpadu Analog (Analog IC)?", "id": "Op-Amp, Regulator Tegangan Linear." },
  { "en": "Apa Itu Sirkuit Terpadu Digital (Digital IC)?", "id": "IC (Integrated Circuit) Proses Sinyal Digital." },
  { "en": "Contoh Sirkuit Terpadu Digital (Digital IC)?", "id": "Gerbang Logika, Mikroprosesor, Memori." },
  { "en": "Apa Itu Sirkuit Terpadu Sinyal Campuran (Mixed-Signal IC)?", "id": "IC Gabungkan Fungsi Analog, Digital." },
  { "en": "Contoh Sirkuit Terpadu Sinyal Campuran (Mixed-Signal IC)?", "id": "ADC (Analog-to-Digital Converter), DAC (Digital-to-Analog Converter)." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Tercetak Koneksi Komponen." },
  { "en": "Bahan Dasar PCB (Printed Circuit Board) Umum?", "id": "FR-4 (Fiberglass Diperkuat Epoksi)." },
  { "en": "Apa Itu Jalur (Trace) PCB?", "id": "Garis Tembaga Konduktif Di Papan." },
  { "en": "Apa Itu Pad (Pad) PCB?", "id": "Area Tembaga Tempat Solder Kaki Komponen." },
  { "en": "Apa Itu Via (Via) PCB?", "id": "Lubang Konduktif Penghubung Lapisan PCB." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan Pelindung Jalur PCB (Biasanya Hijau)." },
  { "en": "Apa Itu Silkscreen (Sablon Sutra)?", "id": "Teks Dan Simbol Identifikasi Komponen." },
  { "en": "Apa Itu Komponen Through-Hole (Tembus Lubang)?", "id": "Komponen Kaki Menembus Lubang PCB." },
  { "en": "Apa Itu Komponen Surface Mount (SMD/SMT)?", "id": "Komponen Dipasang Permukaan PCB." },
  { "en": "Keuntungan Komponen SMD (Surface Mount Device)?", "id": "Ukuran Lebih Kecil, Kepadatan Tinggi." },
  { "en": "Apa Itu Footprint (Jejak Kaki) Komponen?", "id": "Pola Pad Pada PCB Untuk Komponen." },
  { "en": "Apa Itu File Gerber (Gerber File)?", "id": "Format Standar Manufaktur PCB." },
  { "en": "Apa Itu Solder (Soldering)?", "id": "Proses Penyambungan Logam Menggunakan Timah." },
  { "en": "Apa Fungsi Fluks (Flux) Solder?", "id": "Membersihkan Oksida, Membantu Pembasahan." },
  { "en": "Apa Itu Sambungan Dingin (Cold Solder Joint)?", "id": "Solderan Tidak Menyatu Sempurna (Buruk)." },
  { "en": "Apa Itu Jembatan Solder (Solder Bridge)?", "id": "Timah Hubung Singkat Antar Titik Dekat." },
  { "en": "Apa Itu Desoldering (Desoldering)?", "id": "Proses Melepas Sambungan Solder." },
  { "en": "Alat Bantu Desoldering (Desoldering)?", "id": "Solder Sucker, Solder Wick." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Merusak Komponen." },
  { "en": "Bagaimana Mencegah Kerusakan ESD (Electrostatic Discharge)?", "id": "Gunakan Gelang Anti-Statis, Alas Kerja." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Penentuan Nilai Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Nilai Ukur Ke Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Kedekatan Pengukuran Berulang Satu Sama Lain." },
  { "en": "Apa Itu Resolusi (Resolution)?", "id": "Perubahan Terkecil Dapat Diukur Alat." },
  { "en": "Apa Itu Multimeter Digital (DMM)?", "id": "Alat Ukur Listrik Dasar Digital." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang Listrik." },
  { "en": "Sumbu Vertikal (Vertical Axis) Osiloskop Mewakili Apa?", "id": "Tegangan (Voltage)." },
  { "en": "Sumbu Horizontal (Horizontal Axis) Osiloskop Mewakili Apa?", "id": "Waktu (Time)." },
  { "en": "Apa Itu Pemicuan (Triggering) Osiloskop?", "id": "Menstabilkan Tampilan Gelombang Berulang." },
  { "en": "Apa Itu Probe (Probe) Osiloskop?", "id": "Kabel Penghubung Osiloskop Ke Rangkaian." },
  { "en": "Apa Itu Generator Fungsi (Function Generator)?", "id": "Penghasil Sinyal Uji (Sinus, Kotak)." },
  { "en": "Apa Itu Catu Daya (Power Supply) Laboratorium?", "id": "Sumber Tegangan/Arus DC Variabel Teregulasi." },
  { "en": "Apa Itu Pemecahan Masalah (Troubleshooting)?", "id": "Proses Mencari, Memperbaiki Kesalahan." },
  { "en": "Apa Itu Skematik (Schematic) Rangkaian?", "id": "Diagram Logis Koneksi Komponen." },
  { "en": "Apa Itu Layout (Tata Letak) PCB?", "id": "Desain Fisik Penempatan Komponen PCB." },
  { "en": "Apa Itu Termodinamika (Thermodynamics)?", "id": "Studi Energi, Panas, Kerja." },
  { "en": "Apa Hukum Pertama Termodinamika?", "id": "Kekekalan Energi (ΔU = Q - W)." },
  { "en": "Apa Hukum Kedua Termodinamika?", "id": "Entropi (Entropy) Semesta Selalu Meningkat." },
  { "en": "Apa Hukum Ketiga Termodinamika?", "id": "Entropi (Entropy) Nol Mutlak Tidak Tercapai." },
  { "en": "Apa Itu Suhu (Temperature)?", "id": "Ukuran Energi Kinetik Rata-Rata Partikel." },
  { "en": "Apa Skala Suhu (Temperature Scale) Absolut?", "id": "Skala Kelvin (K)." },
  { "en": "Berapa Nol Mutlak (Absolute Zero) Kelvin?", "id": "Nol Kelvin (0 K)." },
  { "en": "Apa Itu Panas (Heat) (Q)?", "id": "Transfer Energi Akibat Perbedaan Suhu." },
  { "en": "Apa Satuan Panas (Heat)?", "id": "Joule (J) Atau Kalori (Cal)." },
  { "en": "Apa Itu Kerja (Work) (W) Termodinamika?", "id": "Transfer Energi Mekanis Sistem." },
  { "en": "Apa Itu Energi Internal (Internal Energy) (U)?", "id": "Total Energi Kinetik, Potensial Partikel." },
  { "en": "Apa Itu Entropi (Entropy) (S)?", "id": "Ukuran Ketidakteraturan Atau Keacakan." },
  { "en": "Apa Itu Entalpi (Enthalpy) (H)?", "id": "Ukuran Kandungan Panas Total Sistem." },
  { "en": "Apa Itu Energi Bebas Gibbs (Gibbs Free Energy) (G)?", "id": "Menentukan Kespontanan Proses (Tekanan Tetap)." },
  { "en": "Proses Apa Yang Spontan (Menurut Gibbs)?", "id": "Proses Dengan Perubahan ΔG Negatif." },
  { "en": "Apa Itu Kapasitas Panas (Heat Capacity)?", "id": "Jumlah Panas Ubah Suhu Benda 1 Derajat." },
  { "en": "Apa Itu Panas Jenis (Specific Heat)?", "id": "Kapasitas Panas Per Satuan Massa." },
  { "en": "Apa Itu Panas Laten (Latent Heat)?", "id": "Panas Diserap/Dilepas Saat Perubahan Fasa." },
  { "en": "Apa Tiga Mekanisme Perpindahan Panas?", "id": "Konduksi, Konveksi, Radiasi." },
  { "en": "Apa Itu Konduksi (Conduction)?", "id": "Transfer Panas Kontak Langsung Partikel." },
  { "en": "Apa Itu Konveksi (Convection)?", "id": "Transfer Panas Aliran Massa Fluida." },
  { "en": "Apa Itu Radiasi (Radiation) Termal?", "id": "Transfer Panas Gelombang Elektromagnetik." },
  { "en": "Apa Itu Mesin Kalor (Heat Engine)?", "id": "Mengubah Energi Panas Menjadi Kerja Mekanis." },
  { "en": "Apa Itu Siklus Carnot (Carnot Cycle)?", "id": "Siklus Termodinamika Paling Efisien (Teoretis)." },
  { "en": "Apa Efisiensi (Efficiency) Mesin Kalor?", "id": "Rasio Kerja Output Terhadap Input Panas." },
  { "en": "Apa Itu Pompa Kalor (Heat Pump)?", "id": "Memindahkan Panas Dari Dingin Ke Panas (Butuh Kerja)." },
  { "en": "Apa Itu Koefisien Kinerja (COP)?", "id": "Ukuran Efisiensi Pompa Kalor/Pendingin." },
  { "en": "Apa Itu Optika (Optics)?", "id": "Studi Cahaya Dan Perilakunya." },
  { "en": "Apa Sifat Dualisme (Dualism) Cahaya?", "id": "Sebagai Gelombang Dan Partikel (Foton)." },
  { "en": "Apa Itu Pemantulan (Reflection)?", "id": "Cahaya Memantul Dari Permukaan Batas." },
  { "en": "Apa Itu Pembiasan (Refraction)?", "id": "Pembelokan Cahaya Saat Lewati Medium Berbeda." },
  { "en": "Apa Itu Indeks Bias (Refractive Index)?", "id": "Ukuran Pembelokan Cahaya Medium." },
  { "en": "Apa Itu Dispersi (Dispersion)?", "id": "Penguraian Cahaya Putih Menjadi Warna." },
  { "en": "Apa Itu Interferensi (Interference) Cahaya?", "id": "Superposisi Gelombang Cahaya Koheren." },
  { "en": "Apa Itu Difraksi (Diffraction) Cahaya?", "id": "Pelenturan Cahaya Sekitar Hambatan Kecil." },
  { "en": "Apa Itu Polarisasi (Polarization) Cahaya?", "id": "Pembatasan Arah Getar Medan Listrik Cahaya." },
  { "en": "Apa Itu Lensa (Lens)?", "id": "Benda Optik Transparan Membiaskan Cahaya." },
  { "en": "Apa Itu Lensa Cembung (Convex Lens)?", "id": "Mengumpulkan (Memfokuskan) Sinar Cahaya." },
  { "en": "Apa Itu Lensa Cekung (Concave Lens)?", "id": "Menyebarkan Sinar Cahaya." },
  { "en": "Apa Itu Jarak Fokus (Focal Length)?", "id": "Jarak Lensa Ke Titik Fokus Utama." },
  { "en": "Apa Itu Cermin (Mirror)?", "id": "Permukaan Memantulkan Cahaya." },
  { "en": "Apa Itu Cermin Datar (Plane Mirror)?", "id": "Menghasilkan Bayangan Maya, Sama Besar." },
  { "en": "Apa Itu Cermin Cekung (Concave Mirror)?", "id": "Mengumpulkan Sinar Pantul." },
  { "en": "Apa Itu Cermin Cembung (Convex Mirror)?", "id": "Menyebarkan Sinar Pantul." },
  { "en": "Apa Itu Mata Manusia (Human Eye)?", "id": "Organ Optik Penglihatan." },
  { "en": "Bagian Mata Apa Memfokuskan Cahaya?", "id": "Kornea Dan Lensa Mata." },
  { "en": "Bagian Mata Apa Menangkap Cahaya?", "id": "Retina (Mengandung Sel Fotoreseptor)." },
  { "en": "Apa Itu Sel Batang (Rod Cell) Retina?", "id": "Peka Cahaya Redup (Penglihatan Malam)." },
  { "en": "Apa Itu Sel Kerucut (Cone Cell) Retina?", "id": "Peka Warna (Penglihatan Siang Hari)." },
  { "en": "Apa Itu Rabun Jauh (Myopia)?", "id": "Sulit Melihat Jauh (Bayangan Depan Retina)." },
  { "en": "Lensa Apa Mengoreksi Rabun Jauh (Myopia)?", "id": "Lensa Cekung (Negatif)." },
  { "en": "Apa Itu Rabun Dekat (Hyperopia)?", "id": "Sulit Melihat Dekat (Bayangan Belakang Retina)." },
  { "en": "Lensa Apa Mengoreksi Rabun Dekat (Hyperopia)?", "id": "Lensa Cembung (Positif)." },
  { "en": "Apa Itu Presbiopi (Presbyopia)?", "id": "Kehilangan Akomodasi Mata Akibat Usia." },
  { "en": "Apa Itu Astigmatisma (Astigmatism)?", "id": "Kelengkungan Kornea Tidak Rata." },
  { "en": "Apa Itu Kamera (Camera)?", "id": "Alat Optik Merekam Gambar." },
  { "en": "Bagian Kamera Serupa Lensa Mata?", "id": "Lensa Kamera." },
  { "en": "Bagian Kamera Serupa Retina Mata?", "id": "Sensor Gambar (Film Atau Digital)." },
  { "en": "Apa Itu Apertur (Aperture) Kamera?", "id": "Bukaan Pengatur Jumlah Cahaya Masuk." },
  { "en": "Apa Itu Kecepatan Rana (Shutter Speed)?", "id": "Durasi Sensor Terpapar Cahaya." },
  { "en": "Apa Itu ISO (ISO) Kamera?", "id": "Sensitivitas Sensor Terhadap Cahaya." },
  { "en": "Apa Itu Mikroskop (Microscope)?", "id": "Alat Memperbesar Objek Sangat Kecil." },
  { "en": "Apa Itu Teleskop (Telescope)?", "id": "Alat Mengamati Objek Sangat Jauh." },
  { "en": "Apa Itu Mekanika Kuantum (Quantum Mechanics)?", "id": "Fisika Mengatur Perilaku Skala Atom." },
  { "en": "Apa Itu Foton (Photon)?", "id": "Kuantum (Partikel) Cahaya." },
  { "en": "Apa Itu Efek Fotolistrik (Photoelectric Effect)?", "id": "Elektron Dipancarkan Logam Disinari Cahaya." },
  { "en": "Apa Itu Dualitas Gelombang-Partikel (Wave-Particle Duality)?", "id": "Partikel Menunjukkan Sifat Gelombang, Sebaliknya." },
  { "en": "Apa Itu Prinsip Ketidakpastian Heisenberg?", "id": "Batas Akurasi Pengukuran Posisi, Momentum." },
  { "en": "Apa Itu Model Atom Bohr (Bohr Model)?", "id": "Model Atom Elektron Orbit Energi Tertentu." },
  { "en": "Apa Itu Bilangan Kuantum (Quantum Number)?", "id": "Angka Deskripsi Keadaan Elektron Atom." },
  { "en": "Apa Itu Spin (Spin) Elektron?", "id": "Momentum Sudut Intrinsik Elektron." },
  { "en": "Apa Itu Penguat Umpan Balik (Feedback Amplifier)?", "id": "Penguat Gunakan Sebagian Output Ke Input." },
  { "en": "Apa Dua Jenis Umpan Balik (Feedback) Utama?", "id": "Umpan Balik Positif Dan Negatif." },
  { "en": "Apa Efek Umpan Balik Negatif (Negative Feedback)?", "id": "Stabilkan Gain, Kurangi Distorsi, Ubah Impedansi." },
  { "en": "Apa Efek Umpan Balik Positif (Positive Feedback)?", "id": "Tingkatkan Gain, Bisa Sebabkan Osilasi." },
  { "en": "Kapan Umpan Balik Positif (Positive Feedback) Digunakan?", "id": "Dalam Rangkaian Osilator, Schmitt Trigger." },
  { "en": "Apa Itu Penguatan Loop (Loop Gain)?", "id": "Total Penguatan Jalur Umpan Balik." },
  { "en": "Syarat Osilasi (Oscillation) Terkait Penguatan Loop?", "id": "Magnitudo Penguatan Loop Sama Dengan Satu." },
  { "en": "Syarat Osilasi (Oscillation) Terkait Fasa Loop?", "id": "Pergeseran Fasa Loop Nol Atau 360 Derajat." },
  { "en": "Apa Itu Kriteria Kestabilan Barkhausen (Barkhausen Criterion)?", "id": "Syarat Matematis Terjadinya Osilasi." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Hasilkan Sinyal Periodik Sendiri." },
  { "en": "Jenis Osilator (Oscillator) Berdasarkan Komponen Frekuensi?", "id": "Osilator RC, LC, Kristal." },
  { "en": "Apa Itu Osilator (Oscillator) RC?", "id": "Gunakan Jaringan Resistor, Kapasitor." },
  { "en": "Contoh Osilator (Oscillator) RC?", "id": "Geser Fasa, Jembatan Wien." },
  { "en": "Apa Itu Osilator (Oscillator) LC?", "id": "Gunakan Jaringan Induktor, Kapasitor (Tank)." },
  { "en": "Contoh Osilator (Oscillator) LC?", "id": "Colpitts, Hartley, Clapp." },
  { "en": "Apa Itu Rangkaian Tank (Tank Circuit)?", "id": "Kombinasi Paralel Induktor, Kapasitor." },
  { "en": "Apa Itu Osilator (Oscillator) Kristal?", "id": "Gunakan Kristal Kuarsa Resonator Frekuensi." },
  { "en": "Apa Keunggulan Osilator (Oscillator) Kristal?", "id": "Stabilitas Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Sifat Kristal Bergetar Medan Listrik." },
  { "en": "Apa Itu Multivibrator (Multivibrator)?", "id": "Osilator Hasilkan Gelombang Non-Sinusoidal." },
  { "en": "Apa Itu Multivibrator Astabil (Astable Multivibrator)?", "id": "Tidak Punya Keadaan Stabil (Osilator Bebas)." },
  { "en": "Apa Itu Multivibrator Monostabil (Monostable Multivibrator)?", "id": "Punya Satu Keadaan Stabil (One-Shot)." },
  { "en": "Apa Itu Multivibrator Bistabil (Bistable Multivibrator)?", "id": "Punya Dua Keadaan Stabil (Flip-Flop)." },
  { "en": "Apa Itu Timer (Timer) IC 555?", "id": "IC Serbaguna Implementasi Multivibrator." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Umpan Balik Sinkronisasi Fasa." },
  { "en": "Apa Fungsi Utama PLL (Phase-Locked Loop)?", "id": "Sintesis Frekuensi, Demodulasi, Pemulihan Clock." },
  { "en": "Komponen Utama PLL (Phase-Locked Loop)?", "id": "Detektor Fasa, Filter Loop, VCO." },
  { "en": "Apa Itu Voltage-Controlled Oscillator (VCO)?", "id": "Osilator Frekuensinya Dikontrol Tegangan." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier)?", "id": "Penguat Tingkatkan Level Daya Sinyal." },
  { "en": "Apa Itu Kelas (Class) Penguat Daya?", "id": "Klasifikasi Berdasarkan Sudut Konduksi." },
  { "en": "Penguat Kelas A (Class A)?", "id": "Konduksi 360 Derajat (Linearitas Terbaik)." },
  { "en": "Efisiensi (Efficiency) Teoritis Maksimal Kelas A?", "id": "Lima Puluh Persen (50%)." },
  { "en": "Penguat Kelas B (Class B)?", "id": "Konduksi 180 Derajat (Push-Pull)." },
  { "en": "Efisiensi (Efficiency) Teoritis Maksimal Kelas B?", "id": "Tujuh Puluh Delapan Koma Lima Persen (78.5%)." },
  { "en": "Masalah Utama Penguat Kelas B?", "id": "Distorsi Crossover (Crossover Distortion)." },
  { "en": "Penguat Kelas AB (Class AB)?", "id": "Konduksi Sedikit Lebih 180 Derajat." },
  { "en": "Tujuan Penguat Kelas AB (Class AB)?", "id": "Mengatasi Distorsi Crossover Kelas B." },
  { "en": "Penguat Kelas C (Class C)?", "id": "Konduksi Kurang Dari 180 Derajat." },
  { "en": "Efisiensi (Efficiency) Penguat Kelas C?", "id": "Sangat Tinggi (Tapi Sangat Non-Linear)." },
  { "en": "Aplikasi Penguat Kelas C (Class C)?", "id": "Penguat RF (Frekuensi Radio) Tertala." },
  { "en": "Penguat Kelas D (Class D)?", "id": "Penguat Switching (PWM)." },
  { "en": "Efisiensi (Efficiency) Penguat Kelas D?", "id": "Sangat Tinggi (Bisa > 90%)." },
  { "en": "Aplikasi Penguat Kelas D (Class D)?", "id": "Audio Daya Tinggi, Efisien." },
  { "en": "Apa Itu Efisiensi (Efficiency) Penguat?", "id": "Rasio Daya Output AC Ke Input DC." },
  { "en": "Apa Itu Disipasi Daya (Power Dissipation)?", "id": "Daya Hilang Jadi Panas Komponen Aktif." },
  { "en": "Apa Itu Heat Sink (Penyerap Panas)?", "id": "Komponen Bantu Pembuangan Panas." },
  { "en": "Mengapa Penguat Daya Perlu Heat Sink?", "id": "Mencegah Kerusakan Akibat Panas Berlebih." },
  { "en": "Apa Itu Penyetaraan Impedansi (Impedance Matching)?", "id": "Menyamakan Impedansi Sumber, Beban." },
  { "en": "Tujuan Penyetaraan Impedansi (Impedance Matching)?", "id": "Transfer Daya Maksimal, Kurangi Pantulan." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Mode Diferensial (Differential Mode) Input?", "id": "Selisih Tegangan Antara Dua Input." },
  { "en": "Apa Itu Mode Bersama (Common Mode) Input?", "id": "Rata-Rata Tegangan Kedua Input." },
  { "en": "Apa Itu Penguatan Diferensial (Differential Gain) (Ad)?", "id": "Penguatan Sinyal Mode Diferensial." },
  { "en": "Apa Itu Penguatan Mode Bersama (Common-Mode Gain) (Ac)?", "id": "Penguatan Sinyal Mode Bersama." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Rasio Ad Terhadap Ac (Ideal Tak Hingga)." },
  { "en": "Apa Itu Impedansi Input Diferensial (Differential Input Z)?", "id": "Impedansi Antara Dua Terminal Input." },
  { "en": "Apa Itu Impedansi Input Mode Bersama (Common-Mode Input Z)?", "id": "Impedansi Tiap Input Ke Ground." },
  { "en": "Apa Itu Tegangan Offset Input (Input Offset Voltage)?", "id": "Tegangan Diferensial Agar Output Nol." },
  { "en": "Apa Itu Arus Bias Input (Input Bias Current)?", "id": "Arus DC Rata-Rata Masuk Input Op-Amp." },
  { "en": "Apa Itu Arus Offset Input (Input Offset Current)?", "id": "Selisih Arus Bias Antara Dua Input." },
  { "en": "Apa Itu Slew Rate (Slew Rate) Op-Amp?", "id": "Laju Perubahan Tegangan Output Maksimal." },
  { "en": "Satuan Slew Rate (Slew Rate)?", "id": "Volt Per Mikrosekon (V/µs)." },
  { "en": "Apa Itu Gain Bandwidth Product (GBP)?", "id": "Hasil Kali Gain Loop Tertutup, Bandwidth." },
  { "en": "Apa Itu Virtual Ground (Tanah Virtual)?", "id": "Input Inverting Dianggap 0V (Feedback Negatif)." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Menggunakan Op-Amp." },
  { "en": "Apa Keuntungan Filter Aktif (Active Filter)?", "id": "Gain, Isolasi Beban, Tanpa Induktor." },
  { "en": "Apa Itu Topologi Sallen-Key (Sallen-Key Topology)?", "id": "Topologi Filter Aktif Umum (Orde Dua)." },
  { "en": "Apa Itu Topologi Multiple Feedback (MFB)?", "id": "Topologi Filter Aktif Lain (Inverting)." },
  { "en": "Apa Itu Filter State Variable (State Variable Filter)?", "id": "Filter Aktif Hasilkan Output LP, HP, BP." },
  { "en": "Apa Itu Filter Biquad (Biquad Filter)?", "id": "Filter Aktif Orde Dua (Topologi Umum)." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Batas Respon Filter (-3dB)." },
  { "en": "Apa Itu Respon Butterworth (Butterworth Response)?", "id": "Passband Datar Maksimal." },
  { "en": "Apa Itu Respon Chebyshev (Chebyshev Response)?", "id": "Roll-Off Curam, Riak Di Passband." },
  { "en": "Apa Itu Respon Bessel (Bessel Response)?", "id": "Respon Fasa Linear Terbaik (Delay Rata)." },
  { "en": "Apa Itu Respon Eliptik (Elliptic Response)?", "id": "Roll-Off Paling Curam, Riak Passband, Stopband." },
  { "en": "Apa Nama Lain Respon Eliptik (Elliptic Response)?", "id": "Filter Cauer (Cauer Filter)." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Besaran Analog Ke Digital." },
  { "en": "Apa Itu Resolusi (Resolution) ADC?", "id": "Jumlah Bit Representasi Digital." },
  { "en": "Apa Itu Laju Sampel (Sample Rate) ADC?", "id": "Jumlah Konversi Per Detik." },
  { "en": "Apa Itu Kesalahan Kuantisasi (Quantization Error)?", "id": "Error Akibat Pembulatan Nilai Analog." },
  { "en": "Apa Itu Differential Non-Linearity (DNL)?", "id": "Deviasi Lebar Step Kuantisasi Ideal." },
  { "en": "Apa Kepanjangan DNL (Differential Non-Linearity)?", "id": "Non-Linearitas Diferensial." },
  { "en": "Apa Itu Integral Non-Linearity (INL)?", "id": "Deviasi Fungsi Transfer ADC Garis Lurus." },
  { "en": "Apa Kepanjangan INL (Integral Non-Linearity)?", "id": "Non-Linearitas Integral." },
  { "en": "Apa Itu Signal-to-Noise and Distortion Ratio (SINAD)?", "id": "Ukuran Kualitas Sinyal ADC." },
  { "en": "Apa Kepanjangan SINAD (Signal-to-Noise and Distortion Ratio)?", "id": "Rasio Sinyal Terhadap Derau Dan Distorsi." },
  { "en": "Apa Itu Effective Number of Bits (ENOB)?", "id": "Ukuran Resolusi Aktual ADC (Perhitungkan Noise)." },
  { "en": "Apa Itu Spurious Free Dynamic Range (SFDR)?", "id": "Rasio Sinyal Fundamental Terhadap Spurious Terbesar." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Flash?", "id": "Sangat Cepat (Banyak Komparator Paralel)." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) SAR?", "id": "Successive Approximation Register (Umum)." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Sigma-Delta?", "id": "Resolusi Tinggi (Oversampling, Noise Shaping)." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Besaran Digital Ke Analog." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) R-2R Ladder?", "id": "Struktur Jaringan Resistor Presisi." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) String?", "id": "Menggunakan Rangkaian Pembagi Tegangan Resistor." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) Sigma-Delta?", "id": "Menggunakan Modulasi Kepadatan Pulsa." },
  { "en": "Apa Itu Settling Time (Waktu Turun) DAC?", "id": "Waktu Output Capai Nilai Akhir Stabil." },
  { "en": "Apa Itu Glitch Energy (Energi Glitch) DAC?", "id": "Energi Spike Saat Transisi Kode." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Kontrol Umpan Balik Fasa." },
  { "en": "Aplikasi PLL (Phase-Locked Loop)?", "id": "Sintesis Frekuensi, Demodulasi, Pemulihan Clock." },
  { "en": "Apa Itu Direct Digital Synthesis (DDS)?", "id": "Teknik Sintesis Frekuensi Digital Langsung." },
  { "en": "Apa Keunggulan DDS (Direct Digital Synthesis)?", "id": "Resolusi Tinggi, Switching Cepat, Fasa Kontinu." },
  { "en": "Apa Itu Osilator Terkendali Tegangan (VCO)?", "id": "Frekuensi Output Tergantung Tegangan Kontrol." },
  { "en": "Apa Itu Osilator Terkendali Kristal (XO)?", "id": "Osilator Frekuensi Tetap Sangat Stabil." },
  { "en": "Apa Itu Osilator Terkendali Tegangan Kristal (VCXO)?", "id": "Frekuensi Sedikit Diatur Tegangan." },
  { "en": "Apa Itu Osilator Terkompensasi Suhu Kristal (TCXO)?", "id": "Stabilitas Frekuensi Terhadap Suhu Ditingkatkan." },
  { "en": "Apa Itu Osilator Kontrol Oven Kristal (OCXO)?", "id": "Kristal Dalam Oven Suhu Stabil." },
  { "en": "Apa Satuan Dasar Untuk Impedansi (Impedance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Satuan Dasar Untuk Admitansi (Admittance)?", "id": "Siemens (S)." },
  { "en": "Apa Satuan Dasar Untuk Reaktansi (Reactance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Satuan Dasar Untuk Suseptansi (Susceptance)?", "id": "Siemens (S)." },
  { "en": "Apa Simbol Untuk Impedansi (Impedance)?", "id": "Z." },
  { "en": "Apa Simbol Untuk Admitansi (Admittance)?", "id": "Y." },
  { "en": "Apa Simbol Untuk Reaktansi (Reactance)?", "id": "X." },
  { "en": "Apa Simbol Untuk Suseptansi (Susceptance)?", "id": "B." },
  { "en": "Apa Hubungan Impedansi (Z) Dan Admitansi (Y)?", "id": "Y Sama Dengan 1 / Z." },
  { "en": "Apa Bagian Riil (Real Part) Impedansi?", "id": "Resistansi (Resistance) (R)." },
  { "en": "Apa Bagian Imajiner (Imaginary Part) Impedansi?", "id": "Reaktansi (Reactance) (X)." },
  { "en": "Apa Bagian Riil (Real Part) Admitansi?", "id": "Konduktansi (Conductance) (G)." },
  { "en": "Apa Bagian Imajiner (Imaginary Part) Admitansi?", "id": "Suseptansi (Susceptance) (B)." },
  { "en": "Apa Itu Daya Kompleks (Complex Power) (S)?", "id": "Representasi Fasor Daya AC." },
  { "en": "Apa Rumus Daya Kompleks (Complex Power) (S)?", "id": "S Sama Dengan P + Jq." },
  { "en": "Apa Daya Nyata (Real Power) (P)?", "id": "Daya Disipasi Rata-Rata." },
  { "en": "Apa Daya Reaktif (Reactive Power) (Q)?", "id": "Daya Bolak-Balik Komponen Reaktif." },
  { "en": "Apa Daya Semu (Apparent Power) (|S|)?", "id": "Magnitudo Total Daya AC." },
  { "en": "Apa Faktor Daya (Power Factor) (PF)?", "id": "Rasio Daya Nyata, Daya Semu." },
  { "en": "Faktor Daya (Power Factor) Beban Resistif Murni?", "id": "Satu (1)." },
  { "en": "Faktor Daya (Power Factor) Beban Induktif Murni?", "id": "Nol Lagging (0 Lagging)." },
  { "en": "Faktor Daya (Power Factor) Beban Kapasitif Murni?", "id": "Nol Leading (0 Leading)." },
  { "en": "Apa Itu Konversi Sumber (Source Transformation)?", "id": "Mengubah Sumber Tegangan Ke Arus (Ekuivalen)." },
  { "en": "Apa Itu Teorema Superposisi (Superposition Theorem)?", "id": "Respon Total Jumlah Respon Individual Sumber." },
  { "en": "Apa Syarat Berlaku Superposisi (Superposition)?", "id": "Rangkaian Harus Bersifat Linear." },
  { "en": "Apa Itu Teorema Thevenin (Thevenin's Theorem)?", "id": "Sederhanakan Rangkaian Jadi Vth, Rth Seri." },
  { "en": "Apa Itu Teorema Norton (Norton's Theorem)?", "id": "Sederhanakan Rangkaian Jadi In, Rn Paralel." },
  { "en": "Apa Hubungan Rth Dan Rn?", "id": "Rth Sama Dengan Rn." },
  { "en": "Apa Hubungan Vth, In, Rth?", "id": "Vth Sama Dengan In Kali Rth." },
  { "en": "Apa Itu Transfer Daya Maksimum (Maximum Power Transfer)?", "id": "Kondisi Beban Terima Daya Maksimal." },
  { "en": "Syarat Transfer Daya Maksimum (Maximum Power Transfer) DC?", "id": "Resistansi Beban Sama Dengan Thevenin." },
  { "en": "Syarat Transfer Daya Maksimum (Maximum Power Transfer) AC?", "id": "Impedansi Beban Konjugat Thevenin." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Perilaku Rangkaian Terhadap Frekuensi Berbeda." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Logaritmik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency)?", "id": "Frekuensi Daya Turun Setengah (-3dB)." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Operasi Efektif." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Kondisi Reaktansi Saling Menghilangkan." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Ukuran Rasio Energi Tersimpan, Hilang." },
  { "en": "Q (Faktor Kualitas) Tinggi Berarti Resonansi Bagaimana?", "id": "Resonansi Sangat Tajam (Selektif)." },
  { "en": "Apa Itu Filter Pasif (Passive Filter)?", "id": "Filter Komponen Pasif (R, L, C)." },
  { "en": "Apa Itu Filter Aktif (Active Filter)?", "id": "Filter Komponen Aktif (Op-Amp)." },
  { "en": "Apa Itu Dioda (Diode) Ideal?", "id": "Saklar Sempurna Satu Arah." },
  { "en": "Apa Tegangan Cut-In (Cut-In Voltage) Dioda Silikon?", "id": "Sekitar Nol Koma Tujuh Volt (0.7V)." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Pengubah AC Menjadi DC." },
  { "en": "Apa Itu Filter Kapasitor (Capacitor Filter)?", "id": "Menghaluskan Tegangan DC Hasil Penyearahan." },
  { "en": "Apa Itu Regulator Zener (Zener Regulator)?", "id": "Penstabil Tegangan Menggunakan Dioda Zener." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Perangkat Tiga Terminal Terkontrol Arus." },
  { "en": "Apa Itu Penguatan Arus (Current Gain) (Beta) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Itu Daerah Aktif (Active Region) BJT?", "id": "Daerah Operasi Penguatan Linear." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Perangkat Tiga Terminal Terkontrol Tegangan." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET Dengan Gerbang Terisolasi Oksida." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Impedansi Input (Input Impedance) Op-Amp Ideal?", "id": "Tak Terhingga." },
  { "en": "Apa Penguatan (Gain) Op-Amp Loop Terbuka?", "id": "Sangat Besar (Ideal Tak Hingga)." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Penguatan Op-Amp." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Penguat Op-Amp Dengan Gain = 1." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Syarat Osilasi (Oscillation)?", "id": "Kriteria Barkhausen Terpenuhi." },
  { "en": "Apa Itu Logika Digital (Digital Logic)?", "id": "Logika Berbasis Nilai Diskrit (0, 1)." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Bangunan Sirkuit Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Untuk Logika Digital." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Dasar Digital." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Tegangan AC Dengan Induksi." },
  { "en": "Apa Itu Efisiensi (Efficiency) Transformator?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Itu Motor Induksi (Induction Motor)?", "id": "Motor AC Asinkron Paling Umum." },
  { "en": "Apa Itu Slip (Slip) Motor Induksi?", "id": "Perbedaan Kecepatan Rotor, Medan Putar." },
  { "en": "Apa Itu Generator Sinkron (Synchronous Generator)?", "id": "Pembangkit Utama Listrik AC (Alternator)." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Jaringan Listrik Pembangkit Hingga Beban." },
  { "en": "Apa Itu Transmisi (Transmission) Tegangan Tinggi?", "id": "Penyaluran Daya Jarak Jauh Efisien." },
  { "en": "Apa Itu Distribusi (Distribution) Tegangan Rendah?", "id": "Penyaluran Daya Ke Konsumen." },
  { "en": "Apa Itu Proteksi Arus Lebih (Overcurrent Protection)?", "id": "Perlindungan Terhadap Arus Berlebih." },
  { "en": "Apa Itu Ground Fault (Gangguan Tanah)?", "id": "Aliran Arus Tak Diinginkan Ke Tanah." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Aliran Daya Semikonduktor." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Kontrol Rata-Rata Tegangan/Arus." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback) Kontrol?", "id": "Menggunakan Output Mengoreksi Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Digital Industri Fleksibel." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Global Antar Jaringan." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Pengenal Unik Perangkat Di Jaringan IP." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Interaksi Listrik Dan Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Transduser Gelombang EM Dan Listrik." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Reproduktifitas Hasil Pengukuran." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik V, A, Ω." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Visualisasi Bentuk Gelombang Listrik." },
  { "en": "Apa Itu Sensor Suhu (Temperature Sensor)?", "id": "Mengukur Derajat Panas Dingin." },
  { "en": "Apa Itu Sensor Cahaya (Light Sensor)?", "id": "Mengukur Intensitas Cahaya." },
  { "en": "Apa Itu Sensor Jarak (Distance Sensor)?", "id": "Mengukur Jarak Ke Objek." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Penggerak Putar Arus Searah." },
  { "en": "Apa Itu Motor AC (Alternating Current)?", "id": "Penggerak Putar Arus Bolak-Balik." },
  { "en": "Apa Itu Relay (Relay)?", "id": "Saklar Elektromekanis." },
  { "en": "Apa Itu Keselamatan Listrik (Electrical Safety)?", "id": "Pencegahan Bahaya Akibat Listrik." },
  { "en": "Apa Bahaya Utama Listrik?", "id": "Sengatan, Kebakaran, Busur Api." },
  { "en": "Apa Fungsi Pembumian (Grounding) Keselamatan?", "id": "Menyediakan Jalur Aman Arus Bocor." },
  { "en": "Apa Itu Isolasi (Insulation)?", "id": "Bahan Pencegah Kontak Konduktor." },
  { "en": "Peralatan Kelas I (Class I) Memerlukan Apa?", "id": "Koneksi Pembumian (Ground) Pengaman." },
  { "en": "Peralatan Kelas II (Class II) Memiliki Apa?", "id": "Isolasi Ganda Atau Diperkuat." },
  { "en": "Apa Simbol Isolasi Ganda (Double Insulation)?", "id": "Kotak Di Dalam Kotak." },
  { "en": "Apa Itu GFCI/RCCB (Residual Current Device)?", "id": "Pelindung Terhadap Arus Bocor Tanah." },
  { "en": "Apa Prinsip Kerja GFCI/RCCB?", "id": "Mendeteksi Perbedaan Arus Fasa, Netral." },
  { "en": "Apa Itu Lockout/Tagout (LOTO)?", "id": "Prosedur Pengamanan Saat Perbaikan." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Ukuran Kemudahan Hantarkan Arus." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Ukuran Hambatan Jenis Material." },
  { "en": "Apa Satuan Konduktivitas (Conductivity)?", "id": "Siemens Per Meter (S/m)." },
  { "en": "Apa Satuan Resistivitas (Resistivity)?", "id": "Ohm Meter (Ω·m)." },
  { "en": "Bahan Apa Konduktor Terbaik Umum?", "id": "Perak (Silver)." },
  { "en": "Bahan Apa Isolator Terbaik Umum?", "id": "Udara Kering, Kaca, Porselen." },
  { "en": "Apa Pengaruh Suhu Pada Konduktivitas Logam?", "id": "Konduktivitas Umumnya Menurun." },
  { "en": "Apa Pengaruh Suhu Pada Konduktivitas Semikonduktor?", "id": "Konduktivitas Umumnya Meningkat." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
