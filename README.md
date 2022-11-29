const express = require('express');
const ioxy = express();
const wall = require('express-rate-limit');
const path = require('path');
const gradient = require('gradient-string');
const { exit } = require('process');

const titan = wall({
    max: 1000000000,
    message: 'Terlalu banyak permintaan koneksi, coba lagi nanti ...',
    windowMs: 15 * 60 * 100
});

async function ioxys_start() {
    const p = require('prompt-sync')();
    const ip = p(gradient.fruit("[*] IP Address : "));
    const maintenancemessage = p(gradient.fruit("[*] Maintenance message : "));
    const nyalainmt = p(gradient.fruit("[*] Maintenance Mode (y/n) : "));

    ioxy.get('/', titan, function(req, res) {
        res.sendFile(path.join(__dirname + '/home.html'));
    });

    if (nyalainmt == "y") {
        ioxy.post("/growtopia/server_data.php", titan, function(req, res) {
            res.send(`server|${ip}\nport|17091\ntype|1\nmaint|\`#${maintenancemessage}\n\nbeta_server|127.0.0.1\nbeta_port|17091\n\nbeta_type|1\nmeta|localhost\nRTENDMARKERBS1001`)
        });
        async function console_y() {
            console.clear();
            console.log(gradient.rainbow(`[INFO] HTTP PORT : 80\n[INFO] Maintenance Mode : YES\n[INFO] Maintenance Message : ${maintenancemessage}`))
        }
        await ioxy.listen(80, console_y);
    } else if (nyalainmt == "n") {
        ioxy.post("/growtopia/server_data.php", titan, function(req, res) {
            res.send(`server|${ip}\nport|17091\ntype|1\n#maint|\`#Server is under maintenance. We will be back online shortly, because there is a fix that must be fixed soon, thank you for playing on our server!\n\nbeta_server|127.0.0.1\nbeta_port|17091\n\nbeta_type|1\nmeta|localhost\nRTENDMARKERBS1001`)
        });
        async function console_n() {
            console.clear();
            console.log(gradient.rainbow(`[INFO] HTTP PORT : 80\n[INFO] Maintenance Mode : NO\n[INFO] Maintenance Message : ${maintenancemessage}`))
        }
        await ioxy.listen(80, console_n);
    }
    else {
        console.log(gradient.fruit(`\n[*] Terjadi kesalahan dalam menjalankan aplikasi, silahkan coba lagi`));
        exit();
    }
};

ioxys_start();

/*
Server is under maintenance. We will be back online shortly, because there is a fix that must be fixed soon, thank you for playing on our server!
*/
