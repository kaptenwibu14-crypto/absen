[index_Version5.html](https://github.com/user-attachments/files/22246923/index_Version5.html)[Uploading index<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Absensi Siswa</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #111827;
        }
        .container {
            background-color: #1f2937;
            border-radius: 12px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.2);
        }
        .form-input {
            border: 1px solid #4b5563;
            background-color: #374151;
            color: #d1d5db;
            font-size: 1rem;
        }
        .btn-primary {
            background-color: #fff;
            color: #1f2937;
            transition: background 0.2s, color 0.2s;
        }
        .btn-primary:hover {
            background-color: #e5e7eb;
            color: #111827;
        }
        .table-header {
            background-color: #374151;
        }
        .table-image {
            max-width: 60px;
            max-height: 60px;
            object-fit: cover;
        }
        input[type="radio"].form-radio:checked {
            accent-color: #fff;
        }
        .form-input:focus, .btn-primary:focus {
            outline: 2px solid #fff !important;
            outline-offset: 2px !important;
            box-shadow: none !important;
        }
        @media (max-width: 640px) {
            .container {
                padding: 1rem !important;
            }
            h1 {
                font-size: 1.5rem;
            }
            h2 {
                font-size: 1.1rem;
            }
            .form-input, .btn-primary {
                font-size: 1rem;
                padding: 0.75rem 1rem;
            }
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-2 sm:p-4 bg-gray-900">
    <div class="container w-full max-w-4xl mx-auto space-y-8 p-4 sm:p-8">
        <h1 class="text-2xl sm:text-3xl font-bold text-center text-gray-100 mb-2">Sistem Absensi Siswa</h1>

        <!-- Formulir Absensi -->
        <div class="space-y-4">
            <h2 class="text-xl sm:text-2xl font-semibold text-gray-200">Formulir Absensi</h2>
            <form id="absenForm" class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                <!-- Nama Siswa -->
                <div class="space-y-2">
                    <label for="nama" class="block text-sm font-medium text-gray-400">Nama Siswa</label>
                    <input type="text" id="nama" class="form-input rounded-md w-full focus:outline-none" placeholder="Masukkan nama siswa" required>
                </div>
                <!-- Kelas -->
                <div class="space-y-2">
                    <label for="kelas" class="block text-sm font-medium text-gray-400">Kelas</label>
                    <input type="text" id="kelas" class="form-input rounded-md w-full focus:outline-none" placeholder="Contoh: 10A" required>
                </div>
                <!-- Surat Keterangan (Unggah File) -->
                <div class="space-y-2 sm:col-span-2">
                    <label for="suratKeteranganFile" class="block text-sm font-medium text-gray-400">Unggah Foto Surat Keterangan (Opsional)</label>
                    <input type="file" id="suratKeteranganFile" accept="image/*" class="form-input rounded-md w-full focus:outline-none">
                </div>
                <!-- Keterangan -->
                <div class="sm:col-span-2 space-y-2">
                    <label class="block text-sm font-medium text-gray-400">Keterangan</label>
                    <div class="flex items-center space-x-8">
                        <label class="inline-flex items-center text-gray-400">
                            <input type="radio" name="keterangan" value="Hadir" class="form-radio rounded-full" checked>
                            <span class="ml-2">Hadir</span>
                        </label>
                        <label class="inline-flex items-center text-gray-400">
                            <input type="radio" name="keterangan" value="Izin" class="form-radio rounded-full">
                            <span class="ml-2">Izin</span>
                        </label>
                    </div>
                </div>
                <!-- Tombol Simpan -->
                <div class="sm:col-span-2">
                    <button type="submit" class="w-full px-4 py-2 font-semibold rounded-md btn-primary focus:outline-none mt-2">Simpan Absensi</button>
                </div>
            </form>
            <!-- Message Box -->
            <div id="messageBox" class="hidden p-3 mt-2 text-sm text-center rounded-md"></div>
        </div>

        <!-- Tabel Absensi -->
        <div>
            <h2 class="text-xl sm:text-2xl font-semibold text-gray-200 mb-2">Data Absensi Hari Ini</h2>
            <div class="overflow-x-auto rounded-md border border-gray-700">
                <table class="min-w-full divide-y divide-gray-700 text-xs sm:text-sm">
                    <thead class="table-header">
                        <tr>
                            <th class="px-2 sm:px-6 py-2 sm:py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">Tanggal</th>
                            <th class="px-2 sm:px-6 py-2 sm:py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">Nama</th>
                            <th class="px-2 sm:px-6 py-2 sm:py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">Kelas</th>
                            <th class="px-2 sm:px-6 py-2 sm:py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">Keterangan</th>
                            <th class="px-2 sm:px-6 py-2 sm:py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">Surat Keterangan</th>
                            <th class="px-2 sm:px-6 py-2 sm:py-3 text-left text-xs font-medium text-gray-400 uppercase tracking-wider">Aksi</th>
                        </tr>
                    </thead>
                    <tbody id="absensiTableBody" class="bg-gray-800 divide-y divide-gray-700">
                        <!-- Data will be inserted here by JavaScript -->
                    </tbody>
                </table>
            </div>
            <div class="flex justify-end mt-4">
                <button id="btnExport" class="px-4 py-2 font-semibold rounded-md btn-primary focus:outline-none text-xs sm:text-base">Ekspor ke Excel</button>
            </div>
        </div>
    </div>

    <script>
        // DOM Elements
        const absenForm = document.getElementById('absenForm');
        const namaInput = document.getElementById('nama');
        const kelasInput = document.getElementById('kelas');
        const suratKeteranganFile = document.getElementById('suratKeteranganFile');
        const absensiTableBody = document.getElementById('absensiTableBody');
        const messageBox = document.getElementById('messageBox');
        const btnExport = document.getElementById('btnExport');

        // Data Absensi dari localStorage
        let dataAbsensi = JSON.parse(localStorage.getItem('absensiData')) || [];

        // Tampilkan pesan
        function showMessage(msg, type = 'error') {
            messageBox.textContent = msg;
            messageBox.classList.remove('hidden', 'text-red-700', 'bg-red-100', 'border-red-400', 'text-green-700', 'bg-green-100', 'border-green-400');
            if (type === 'error') {
                messageBox.classList.add('text-red-700', 'bg-red-100', 'border', 'border-red-400');
            } else {
                messageBox.classList.add('text-green-700', 'bg-green-100', 'border', 'border-green-400');
            }
            setTimeout(() => messageBox.classList.add('hidden'), 3000);
        }

        // Render tabel absensi
        function renderTable() {
            absensiTableBody.innerHTML = dataAbsensi.map((data, idx) => `
                <tr class="border-b border-gray-700">
                    <td class="px-2 sm:px-6 py-2 whitespace-nowrap text-xs sm:text-sm text-gray-300">${data.tanggal}</td>
                    <td class="px-2 sm:px-6 py-2 whitespace-nowrap text-xs sm:text-sm font-medium text-gray-100">${data.nama}</td>
                    <td class="px-2 sm:px-6 py-2 whitespace-nowrap text-xs sm:text-sm text-gray-300">${data.kelas}</td>
                    <td class="px-2 sm:px-6 py-2 whitespace-nowrap text-xs sm:text-sm text-gray-300">${data.keterangan}</td>
                    <td class="px-2 sm:px-6 py-2 whitespace-nowrap text-xs sm:text-sm text-gray-300">
                        ${data.imageData ? `<img src="${data.imageData}" class="table-image rounded-md">` : '-'}
                    </td>
                    <td class="px-2 sm:px-6 py-2 whitespace-nowrap text-xs sm:text-sm text-gray-300">
                        <button onclick="hapusData(${idx})" class="text-red-500 hover:underline">Hapus</button>
                    </td>
                </tr>
            `).join('');
        }

        // Hapus data absensi
        window.hapusData = function(idx) {
            if (confirm('Yakin ingin menghapus data ini?')) {
                dataAbsensi.splice(idx, 1);
                localStorage.setItem('absensiData', JSON.stringify(dataAbsensi));
                renderTable();
                showMessage('Data berhasil dihapus!', 'success');
            }
        }

        // Submit Formulir Absensi
        absenForm.addEventListener('submit', function(e) {
            e.preventDefault();
            const nama = namaInput.value.trim();
            const kelas = kelasInput.value.trim();
            const keterangan = absenForm.keterangan.value;
            const tanggal = new Date().toLocaleDateString();

            // Validasi input
            if (!nama || !kelas) {
                showMessage('Nama dan Kelas tidak boleh kosong.', 'error');
                return;
            }

            // Cek duplikat
            const existingEntryIndex = dataAbsensi.findIndex(item => item.nama === nama && item.tanggal === tanggal);
            if (existingEntryIndex !== -1) {
                showMessage('Data absensi untuk siswa ini sudah ada hari ini.', 'error');
                return;
            }

            // Validasi file (jika ada)
            const file = suratKeteranganFile.files[0];
            if (file) {
                if (!file.type.startsWith('image/')) {
                    showMessage('File harus berupa gambar!', 'error');
                    return;
                }
                if (file.size > 2 * 1024 * 1024) { // 2MB
                    showMessage('Ukuran gambar maksimal 2MB!', 'error');
                    return;
                }
            }

            // Proses simpan data
            if (file) {
                const reader = new FileReader();
                reader.onload = function(event) {
                    tambahDataAbsensi(nama, kelas, tanggal, keterangan, event.target.result);
                };
                reader.readAsDataURL(file);
            } else {
                tambahDataAbsensi(nama, kelas, tanggal, keterangan, null);
            }
        });

        // Tambah data ke list dan localStorage
        function tambahDataAbsensi(nama, kelas, tanggal, keterangan, imageData) {
            dataAbsensi.push({ nama, kelas, tanggal, keterangan, imageData });
            localStorage.setItem('absensiData', JSON.stringify(dataAbsensi));
            renderTable();
            showMessage('Data absensi berhasil disimpan!', 'success');
            absenForm.reset();
        }

        // Ekspor ke CSV
        btnExport.addEventListener('click', () => {
            if (dataAbsensi.length === 0) {
                showMessage('Tidak ada data untuk diekspor.', 'error');
                return;
            }
            let csvContent = "data:text/csv;charset=utf-8,";
            csvContent += "Tanggal,Nama,Kelas,Keterangan,Surat Keterangan\n";
            dataAbsensi.forEach(row => {
                let rowData = [row.tanggal, row.nama, row.kelas, row.keterangan, row.imageData ? '[Gambar Terlampir]' : ''].join(",");
                csvContent += rowData + "\n";
            });
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `absensi_${new Date().toLocaleDateString().replace(/\//g, '-')}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            showMessage('Data berhasil diekspor!', 'success');
        });

        // Render table saat halaman dimuat
        renderTable();
    </script>
</body>
</html>_Version5.html…]()
