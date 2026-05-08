<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EXP PLUS - ตรวจสอบข้อมูล</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css">
</head>
<body class="p-4">
    <div class="box">
        <h1 class="title is-4">📌 ยืนยันข้อมูลยา</h1>
        <div class="field">
            <label class="label">ชื่อยา</label>
            <input class="input" type="text" id="drug_name">
        </div>
        <div class="field">
            <label class="label">Lot Number</label>
            <input class="input" type="text" id="lot">
        </div>
        <div class="field">
            <label class="label">วันหมดอายุ (ค.ศ.)</label>
            <input class="input" type="date" id="exp">
        </div>
        <div class="field">
            <label class="label">ห้องยา</label>
            <div class="select is-fullwidth">
                <select id="location">
                    <option value="OPD">ห้องยาผู้ป่วยนอก</option>
                    <option value="IPD">ห้องยาผู้ป่วยใน</option>
                </select>
            </div>
        </div>
        <button class="button is-success is-fullwidth" onclick="save()">บันทึกเข้า Google Sheet</button>
    </div>

    <script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
    <script>
        async function init() {
            await liff.init({ liffId: "ใส่_LIFF_ID_ของคุณตรงนี้" });
            
            // ดึงค่าจาก URL (Auto-fill)
            const params = new URLSearchParams(window.location.search);
            document.getElementById('drug_name').value = params.get('n') || '';
            document.getElementById('lot').value = params.get('l') || '';
            document.getElementById('exp').value = params.get('e') || '';
        }
        init();

        async function save() {
            const data = {
                drug_name: document.getElementById('drug_name').value,
                lot: document.getElementById('lot').value,
                exp: document.getElementById('exp').value,
                location: document.getElementById('location').value,
                user: liff.getDecodedIDToken()?.name || 'Unknown'
            };

            // ส่งกลับไปยัง Google Apps Script (Web App URL)
            await fetch('URL_ของ_APPS_SCRIPT_ที่_DEPLOY_แบบ_WEB_APP', {
                method: 'POST',
                body: JSON.stringify(data)
            });

            liff.sendMessages([{ type: 'text', text: `✅ บันทึก ${data.drug_name} แล้ว` }]);
            liff.closeWindow();
        }
    </script>
</body>
</html>
