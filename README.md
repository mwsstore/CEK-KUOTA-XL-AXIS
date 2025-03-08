<!DOCTYPE html>
<html lang="en">
<head>
    <title>Sidompul Cek Kuota Xl Axis</title>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <meta name="format-detection" content="telephone=no" />
    <meta name="robots" content="index, follow">
    <meta name="author" content="Wira Dwi Susanto">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
    <link rel="stylesheet" href="https://my.kmsp-store.com/assets/css/spinner.css">
    <link href="https://my.kmsp-store.com/assets/build/toastr.css" rel="stylesheet" type="text/css" />
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
    <style>
        body {
            background: linear-gradient(135deg, #032B44, #0097A7);
            font-family: 'Arial', sans-serif;
            color: #333333;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        .container {
            margin-top: 20px;
            margin-bottom: 100px;
            padding: 20px;
            background: #cfe4e5;
            border-radius: 8px;
            transition: transform 0.3s ease-in-out;
            max-width: 90%;
        }
        .container:hover {
            transform: scale(1.05);
        }
        h3 {
            font-weight: bold;
            color: #333333;
        }
        #hasilnya {
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
            padding: 20px;
            margin-top: 20px;
        }
        .alert {
            background: #f1f1f1;
            border: none;
            color: #333333;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 5px;
        }
        .form-control {
            border-radius: 8px;
            box-shadow: none;
            border-color: #dddddd;
            padding: 10px;
            width: 100%;
            margin-bottom: 20px;
        }
        .btn-primary {
            background: linear-gradient(135deg, #6a11cb, #2575fc);
            border: none;
            border-radius: 8px;
            box-shadow: none;
            padding: 10px 20px;
            color: white;
            cursor: pointer;
            transition: background 0.3s ease-in-out;
        }
        .btn-primary:hover {
            background: linear-gradient(135deg, #2575fc, #6a11cb);
        }
        #cover-spin {
            position: fixed;
            width: 100%;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            background-color: rgba(255, 255, 255, 0.7);
            z-index: 9999;
            display: none;
        }
        @-webkit-keyframes spin {
            0% {
                -webkit-transform: rotate(0deg);
            }
            100% {
                -webkit-transform: rotate(360deg);
            }
        }
        @keyframes spin {
            0% {
                transform: rotate(0deg);
            }
            100% {
                transform: rotate(360deg);
            }
        }
        #cover-spin::after {
            content: '';
            display: block;
            position: absolute;
            left: 50%;
            top: 50%;
            width: 40px;
            height: 40px;
            border-style: solid;
            border-color: black;
            border-top-color: transparent;
            border-width: 4px;
            border-radius: 50%;
            -webkit-animation: spin .8s linear infinite;
            animation: spin .8s linear infinite;
            transform: translate(-50%, -50%);
        }
    </style>

</head>
<body>
<div id="cover-spin"></div>
<div class="container atur1">
    <center>
        <h3>CEK KUOTA XL AXIS</h3>
        <b>mwsstore</b>
    </center>
    <br>
    <div class="alert alert-success">
        <strong><i class="fa fa-bullhorn" aria-hidden="true"></i> INFO</strong><br>
        Silahkan Digunakan 
    </div>
    <div id="formnya">
        <div class="form-group">
            <label for="msisdn">Nomor XL/AXIS Kamu:</label>
            <input type="number" class="form-control" id="msisdn" placeholder="Ex: 0878xxx/62878xxx" maxlength="16" required>
        </div>
        <button type="button" id="submitCekKuota" class="btn btn-primary">
            <i class="fa fa-sign-in" aria-hidden="true"></i> Cek
        </button>
        <div id="hasilnya" style="text-align: justify;">
            <p id="packageDetail"></p>
        </div>
    </div><br>
</div>
<script type="text/javascript">
    document.addEventListener('DOMContentLoaded', function() {
        const coverSpin = document.getElementById('cover-spin');
        const submitCekKuota = document.getElementById('submitCekKuota');
        const msisdnInput = document.getElementById('msisdn');
        const hasilnya = document.getElementById('hasilnya');

        function cekKuota() {
            const msisdn = msisdnInput.value;
            if (!msisdn) return;
            coverSpin.style.display = 'block';

            fetch(`https://api.khfy-store.com:8443/cek_kuota?msisdn=${msisdn}`, {
                method: 'GET'
            })
            .then(response => response.json())
            .then(s => {
                coverSpin.style.display = 'none';
                if (s.status === false) {
                    toastr.error(s.message);
                    hasilnya.innerHTML = `<br><font style="color: red;"><b>${s.data.keteranganError}</b></font>`;
                } else if (s.status === true) {
                    toastr.success(s.message);
                    hasilnya.innerHTML = `${s.data.hasil}`;
                    // Scroll ke atas hasilnya setelah ditampilkan
                    hasilnya.scrollIntoView({ behavior: 'smooth', block: 'start' });
                }
            })
            .catch(error => {
                coverSpin.style.display = 'none';
                toastr.error('Terjadi kesalahan. Silakan coba lagi!');
            });
        }

        submitCekKuota.addEventListener('click', cekKuota);
        msisdnInput.addEventListener('keypress', function(e) {
            if (e.which == 13) {
                cekKuota();
            }
        });
    });
</script>
<script src="https://my.kmsp-store.com/assets/toastr.js"></script>
</body>
</html>

<style>
    .whatsapp-button {
        position: fixed;
        bottom: 20px;
        right: 20px;
        z-index: 999;
    }
    .whatsapp-button:hover {
        transform: scale(1.2);
        transition: all 0.3s ease-in-out;
    }
</style>

<a href="https://wa.me/6287748842242" target="_blank" class="whatsapp-button">
    <img src="https://i.ibb.co.com/8Lz5rRsD/images-58.jpg" alt="WhatsApp" width="50" height="50">
</a>
