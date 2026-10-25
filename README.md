<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>首爾 2027 家庭智慧旅遊 App</title>
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }
        body { background: #f8fafc; color: #0f172a; padding-bottom: 60px; }
        .header { background: #1e1b4b; color: white; padding: 12px 16px; position: sticky; top: 0; z-index: 10; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .header h1 { font-size: 18px; margin-bottom: 4px; }
        .header p { font-size: 12px; color: #a5b4fc; }
        .nav { display: flex; background: white; border-bottom: 1px solid #e2e8f0; overflow-x: auto; }
        .nav button { flex: 1; min-width: 70px; padding: 10px 4px; border: none; background: none; font-size: 13px; font-weight: bold; color: #64748b; cursor: pointer; border-bottom: 3px solid transparent; white-space: nowrap; }
        .nav button.active { color: #4338ca; border-bottom-color: #4338ca; }
        .container { max-width: 600px; margin: 0 auto; padding: 12px; }
        .card { background: white; border-radius: 8px; border: 1px solid #e2e8f0; padding: 12px; margin-bottom: 10px; box-shadow: 0 1px 2px rgba(0,0,0,0.05); }
        .day-bar { display: flex; gap: 6px; overflow-x: auto; margin-bottom: 12px; }
        .day-btn { padding: 6px 12px; border-radius: 16px; border: 1px solid #cbd5e1; background: white; font-size: 12px; cursor: pointer; white-space: nowrap; }
        .day-btn.active { background: #4338ca; color: white; border-color: #4338ca; }
        .tag { display: inline-block; padding: 2px 6px; border-radius: 4px; font-size: 11px; font-weight: bold; }
        .tag-blue { background: #e0e7ff; color: #3730a3; }
        .tag-orange { background: #ffedd5; color: #c2410c; }
        .tag-pink { background: #fce7f3; color: #be185d; }
        .btn-link { text-decoration: none; padding: 4px 8px; border-radius: 4px; font-size: 11px; font-weight: bold; background: #ecfdf5; color: #047857; margin-left: 4px; }
        .hidden { display: none !important; }
        .btn-add { background: #059669; color: white; border: none; padding: 6px 12px; border-radius: 6px; font-size: 12px; font-weight: bold; cursor: pointer; }
    </style>
</head>
<body>

    <div class="header">
        <h1>✈️ 首爾家庭之旅 2027</h1>
        <p>2027/01/21 – 2027/01/30 (9天8夜) | 首爾時間: <span id="clock" style="color:#fde047; font-weight:bold;">--:--</span></p>
    </div>

    <div class="nav">
        <button onclick="tab('itinerary')" id="t-itinerary" class="active">📅 行程</button>
        <button onclick="tab('dining')" id="t-dining">🍲 餐廳</button>
        <button onclick="tab('shopping')" id="t-shopping">🛍️ 購物</button>
        <button onclick="tab('expenses')" id="t-expenses">💰 記帳(AUD)</button>
        <button onclick="tab('packing')" id="t-packing">🧳 打包</button>
    </div>

    <div class="container">

        <!-- 行程區塊 -->
        <div id="sec-itinerary">
            <div class="day-bar" id="day-btns"></div>
            <div class="card" style="background:#fffbeb; border-color:#f59e0b;">
                <strong id="day-title" style="color:#b45309; font-size:14px;">Day 1</strong>
                <p id="day-summary" style="font-size:12px; color:#92400e; margin-top:4px;"></p>
            </div>
            <div id="items-list"></div>
        </div>

        <!-- 餐廳區塊 -->
        <div id="sec-dining" class="hidden">
            <div class="card" style="display:flex; justify-content:space-between; align-items:center; background:#ea580c; color:white;">
                <strong>🍲 首爾人氣必吃餐廳</strong>
                <button onclick="addDining()" class="btn-add">+ 新增餐廳</button>
            </div>
            <div id="dining-list"></div>
        </div>

        <!-- 購物區塊 -->
        <div id="sec-shopping" class="hidden">
            <div class="card" style="display:flex; justify-content:space-between; align-items:center; background:#db2777; color:white;">
                <strong>🛍️ 購物地標與選品店</strong>
                <button onclick="addShop()" class="btn-add">+ 新增店家</button>
            </div>
            <div id="shopping-list"></div>
        </div>

        <!-- 記帳區塊 -->
        <div id="sec-expenses" class="hidden">
            <div class="card" style="background:#059669; color:white;">
                <strong>💰 費用記帳 (KRW ⇄ AUD)</strong>
                <p style="font-size:12px; margin-top:4px;">1 AUD = <input type="number" id="ex-rate" value="900" onchange="updateRate()" style="width:50px; text-align:right;"> KRW</p>
            </div>
            <div class="card" style="display:flex; justify-content:space-between; text-align:center;">
                <div><span style="font-size:11px; color:#64748b;">總開銷(AUD)</span><h2 id="total-aud" style="color:#059669;">$0.00</h2></div>
                <div><span style="font-size:11px; color:#64748b;">每人分攤(AUD)</span><h2 id="per-aud" style="color:#3730a3;">$0.00</h2></div>
                <button onclick="addExpense()" class="btn-add" style="align-self:center;">+ 記帳</button>
            </div>
            <div class="card" id="expense-list"></div>
        </div>

        <!-- 打包區塊 -->
        <div id="sec-packing" class="hidden">
            <div class="card" style="display:flex; justify-content:space-between; align-items:center;">
                <strong>🎒 冬季與滑雪裝備備忘</strong>
                <button onclick="addPacking()" class="btn-add">+ 新增</button>
            </div>
            <div class="card" id="packing-list"></div>
        </div>

    </div>

    <script>
        // 載入 9 天完整行程數據
        const data = {
            rate: 900,
            activeDay: 0,
            days: [
                {n:1, date:"01/21", title:"抵達首爾・弘大", summary:"CX410 09:20出發，13:50抵達；入住 L9 Stay In Hongdae。", items:[
                    {t:"12:30", type:"住宿", name:"UH Suite The Myeongdong", loc:"49 Myeongdong 8ga-gil", n:"入住明洞住宿。"},
                    {t:"13:50", type:"交通", name:"抵達首爾仁川機場", loc:"Incheon Airport", n:"航班 CX410；入境後前往弘大住宿。"},
                    {t:"15:00", type:"住宿", name:"Check in L9 Stay In Hongdae", loc:"30 Wausan-ro 29-gil", n:"先辦理入住／寄放行李。"},
                    {t:"16:00", type:"購物", name:"弘大購物街", loc:"Hongdae Street", n:"逛街、服飾店與咖啡店。"},
                    {t:"18:30", type:"晚餐", name:"Dinner - Crab Kingdom 弘大蟹王", loc:"Eoulmadang-ro 136", n:"韓式帝王蟹/醬蟹餐廳。"}
                ]},
                {n:2, date:"01/22", title:"西村／首爾老街區", summary:"西村韓屋街區與特色購物巡禮。", items:[
                    {t:"09:00", type:"早餐", name:"弘大麵包店", loc:"Hongdae", n:"偏好 Google 評分 4.5 以上。"},
                    {t:"10:00", type:"景點", name:"景褔宮 - 光化門月台", loc:"1-67 Sejongno", n:"參觀景福宮。"},
                    {t:"12:30", type:"午餐", name:"通仁市場", loc:"Jahamun-ro 15-gil", n:"體驗銅錢便當。"},
                    {t:"14:00", type:"景點", name:"西村 Seochon 街區", loc:"Tongui-dong", n:"散步、拍照與特色小店。"},
                    {t:"14:00", type:"購物", name:"redmangchi gift shop", loc:"Hyoja-ro 7-gil", n:"文創雜貨禮品店。"},
                    {t:"14:30", type:"購物", name:"VINTAGE UNIVERSE", loc:"Pirundae-ro 2-gil", n:"古著復古服飾店。"},
                    {t:"17:00", type:"購物", name:"JUUNEEDU PAJAMAS BOUTIQUE", loc:"Yeonnam-dong", n:"睡衣精品專賣店。"},
                    {t:"18:30", type:"晚餐", name:"Hotel De GGOODD", loc:"Donggyo-ro 46-gil", n:"特色料理與用餐。"}
                ]},
                {n:3, date:"01/23", title:"弘大退房・明洞入住", summary:"09:30退房、寄放行李；約 13:00前往明洞入住。", items:[
                    {t:"09:00", type:"早餐", name:"Bakery Cafe Aoitori 鹽麵包", loc:"Wausan-ro 29-gil", n:"日式鹽可頌名店。"},
                    {t:"10:00", type:"購物", name:"弘大購物街", loc:"Seogyo-dong", n:"退房前輕鬆逛街。"},
                    {t:"12:30", type:"交通", name:"取行李前往明洞", loc:"Hongdae -> Myeongdong", n:"搭乘計程車/Uber。"},
                    {t:"13:00", type:"午餐", name:"宋粥家 Seowonjuk", loc:"Chungmuro 2-ga", n:"鮮美鮑魚粥早餐/午餐。"},
                    {t:"14:00", type:"購物", name:"hetras Myeongdong", loc:"Myeongdong 6-gil", n:"韓國香氛美妝店。"},
                    {t:"14:30", type:"購物", name:"明洞購物街", loc:"Myeongdong", n:"明洞商圈自由購物。"},
                    {t:"19:00", type:"晚餐", name:"Daol Charcoal Grilling", loc:"Myeongdong 8-gil", n:"炭火烤肉晚餐。"},
                    {t:"20:00", type:"景點", name:"Myeongdong Golden Sauna", loc:"Myeongdong", n:"汗蒸幕放鬆體驗。"}
                ]},
                {n:4, date:"01/24", title:"景福宮・北村・益善洞", summary:"明洞早餐 → 三清洞 → 北村韓屋村 → 益善洞。", items:[
                    {t:"08:30", type:"早餐", name:"明洞早餐", loc:"Myeongdong", n:"偏好 Google 4.5 分以上。"},
                    {t:"09:45", type:"景點", name:"北村八景 Bukchon Palgyeong", loc:"Samcheong-ro", n:"韓屋村散步拍照。"},
                    {t:"10:00", type:"景點", name:"三清洞 Samcheong-dong", loc:"Samcheong-dong", n:"藝術街區巡禮。"},
                    {t:"11:00", type:"購物", name:"CUEREN Samcheong", loc:"Bukchon-ro 5-gil", n:"韓國手工皮鞋。"},
                    {t:"13:00", type:"午餐", name:"Seosulla-gil 西巡邏吉", loc:"Gwonnong-dong", n:"宗廟石牆路風格餐廳。"},
                    {t:"14:00", type:"購物", name:"益善洞韓屋村", loc:"Ikseon-dong", n:"特色巷弄與咖啡店。"},
                    {t:"18:30", type:"晚餐", name:"해천어부 Haecheon Oba", loc:"Supyo-ro 28-gil", n:"巨無霸活海鮮鍋。"},
                    {t:"20:00", type:"景點", name:"清溪川散步", loc:"Cheonggyecheon", n:"夜間散步（視體力）。"}
                ]},
                {n:5, date:"01/25", title:"前往 MONA 龍平滑雪場", summary:"08:45 由明洞站乘 Coach 前往 MONA 龍平滑雪場。", items:[
                    {t:"08:00", type:"住宿", name:"明洞住宿退房", loc:"UH Suite", n:"整理行李確認巴士集合點。"},
                    {t:"08:45", type:"交通", name:"Coach 前往 MONA 龍平滑雪場", loc:"Myeongdong -> Yongpyong", n:"已預約專車往江原道。"},
                    {t:"13:00", type:"住宿", name:"MONA 龍平滑雪場入住", loc:"Yongpyong Resort", n:"辦理入住，租借雪具與購買雪票。"}
                ]},
                {n:6, date:"01/26", title:"MONA 龍平全日滑雪", summary:"全日 skiing 滑雪活動，晚上繼續入住度假村。", items:[
                    {t:"09:00", type:"景點", name:"MONA 龍平全日滑雪", loc:"Yongpyong Resort", n:"全天 skiing 與雪上活動。"},
                    {t:"12:30", type:"午餐", name:"滑雪場內午餐", loc:"Yongpyong Resort", n:"渡假村內用餐與保暖休息。"}
                ]},
                {n:7, date:"01/27", title:"龍平退房・返回明洞", summary:"17:00 乘 Coach 回明洞，預計 20:30 抵達；享用明洞醬蟹。", items:[
                    {t:"11:00", type:"住宿", name:"MONA 龍平退房", loc:"Yongpyong Resort", n:"寄存行李與渡假村自由活動。"},
                    {t:"17:00", type:"交通", name:"Coach 返回明洞", loc:"Yongpyong -> Myeongdong", n:"搭車返回首爾。"},
                    {t:"20:45", type:"晚餐", name:"Odarijip 醬油蟹", loc:"Myeongdong", n:"明洞人氣醬蟹白飯殺手。"}
                ]},
                {n:8, date:"01/28", title:"聖水洞購物・南山塔日落", summary:"遊覽聖水洞；午餐馬鈴薯排骨湯，下午南山塔看日落。", items:[
                    {t:"09:30", type:"早餐", name:"聖水洞咖啡街", loc:"Seongsui-ro", n:"品嚐早晨咖啡。"},
                    {t:"10:30", type:"購物", name:"Miffy Store 聖水店", loc:"Yeonmujang 13-gil", n:"米飛兔主題快閃店。"},
                    {t:"11:00", type:"購物", name:"NYU NYU 聖水店", loc:"Yeonmujang-gil 89", n:"整棟牆飾品與耳環天堂。"},
                    {t:"11:30", type:"購物", name:"Brandy Melville", loc:"Yeonmujang-gil", n:"歐美流行服飾。"},
                    {t:"12:30", type:"午餐", name:"Somunnan 聖水排骨湯", loc:"Yeonmujang-gil 45", n:"名店馬鈴薯排骨湯。"},
                    {t:"15:30", type:"景點", name:"南山首爾塔", loc:"N Seoul Tower", n:"搭纜車上山欣賞日落。"},
                    {t:"18:30", type:"晚餐", name:"Eel Sidae 烤鰻魚", loc:"Namdaemun", n:"南大門韓式烤鰻魚。"}
                ]},
                {n:9, date:"01/29", title:"廣藏市場・東大門・前往機場", summary:"廣藏市場買棉被，東大門購物，20:00 前往仁川機場 T1 SPA。", items:[
                    {t:"09:30", type:"早餐", name:"廣藏市場早餐", loc:"Gwangjang Market", n:"體驗綠豆餅與市場小吃。"},
                    {t:"10:30", type:"購物", name:"廣藏市場買棉被", loc:"Gwangjang Market", n:"購買極輕保暖冬被真空打包。"},
                    {t:"12:00", type:"午餐", name:"대통령삼겹살 大統領烤肉", loc:"Dongdaemun", n:"竹筒熟成烤豬五花。"},
                    {t:"13:30", type:"購物", name:"東大門 Doota Mall", loc:"Dongdaemun", n:"商場最後採購。"},
                    {t:"20:00", type:"交通", name:"前往仁川機場第一航廈", loc:"Incheon Airport", n:"搭乘機場巴士。"},
                    {t:"21:30", type:"休息", name:"SPA at Home", loc:"Incheon Airport T1", n:"登機前淋浴放鬆。"},
                    {t:"01:55", type:"交通", name:"凌晨航班起飛 (1/30)", loc:"Incheon Airport", n:"返回溫暖的家。"}
                ]}
            ],
            dining: [
                {name:"Crab Kingdom (弘大蟹王)", area:"弘大", type:"韓式帝王蟹/醬蟹"},
                {name:"通仁市場 Tongin Market", area:"西村", type:"銅錢便當小吃"},
                {name:"宋粥家 Seowonjuk", area:"明洞", type:"鮮美鮑魚粥"},
                {name:"Daol Charcoal Grilling", area:"明洞", type:"韓式炭火燒肉"},
                {name:"해천어부 Haecheon Oba", area:"益善洞", type:"巨無霸海鮮鍋"},
                {name:"Odarijip 醬油蟹", area:"明洞", type:"招牌醬蟹"},
                {name:"Somunnan 聖水排骨湯", area:"聖水洞", type:"馬鈴薯排骨湯"},
                {name:"대통령삼겹살 大統領烤肉", area:"東大門", type:"竹筒熟成烤肉"}
            ],
            shops: [
                {name:"弘大購物街 Hongdae", area:"弘大", type:"潮流服飾"},
                {name:"redmangchi gift shop", area:"西村", type:"文創選品雜貨"},
                {name:"VINTAGE UNIVERSE", area:"西村", type:"古著服飾"},
                {name:"hetras Myeongdong", area:"明洞", type:"精油香氛專賣"},
                {name:"CUEREN Samcheong", area:"三清洞", type:"韓國手工皮鞋"},
                {name:"Miffy Store 聖水店", area:"聖水洞", type:"主題周邊快閃"},
                {name:"NYU NYU 聖水店", area:"聖水洞", type:"飾品耳環天堂"},
                {name:"廣藏市場 Gwangjang", area:"鍾路", type:"韓式保暖冬被"}
            ],
            expenses: [],
            packing: ["護照 (效期6個月以上)", "澳幣/韓幣現金及信用卡", "羽絨外套 / 防風外套", "發熱衣 / 發熱褲 (多套)", "滑雪手套 / 厚毛帽", "韓國轉接頭 (兩圓孔)", "行動電源 / 常備藥品"]
        };

        // 畫面渲染邏輯
        function render() {
            // 天數按鈕
            const dayBtnsDiv = document.getElementById('day-btns');
            dayBtnsDiv.innerHTML = data.days.map((d, i) => `
                <button onclick="changeDay(${i})" class="day-btn ${i === data.activeDay ? 'active' : ''}">
                    Day ${d.n} (${d.date})
                </button>
            `).join('');

            // 當天標題與摘要
            const day = data.days[data.activeDay];
            document.getElementById('day-title').innerText = `Day ${day.n} (${day.date}) : ${day.title}`;
            document.getElementById('day-summary').innerText = day.summary;

            // 行程列表
            const itemsDiv = document.getElementById('items-list');
            itemsDiv.innerHTML = day.items.map(item => `
                <div class="card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div>
                        <span class="tag tag-blue">${item.t}</span>
                        <strong style="margin-left:4px;">${item.name}</strong>
                        <div style="font-size:11px; color:#64748b; margin-top:2px;">📍 ${item.loc}</div>
                        ${item.n ? `<div style="font-size:11px; color:#b45309; margin-top:2px;">📝 ${item.n}</div>` : ''}
                    </div>
                    <div>
                        <a href="https://map.naver.com/v5/search/${encodeURIComponent(item.name)}" target="_blank" class="btn-link">Naver地圖</a>
                    </div>
                </div>
            `).join('');

            // 餐廳
            document.getElementById('dining-list').innerHTML = data.dining.map(d => `
                <div class="card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><strong>${d.name}</strong> <span class="tag tag-orange">${d.area}</span><div style="font-size:11px; color:#64748b;">${d.type}</div></div>
                    <a href="https://map.naver.com/v5/search/${encodeURIComponent(d.name)}" target="_blank" class="btn-link">地圖</a>
                </div>
            `).join('');

            // 購物
            document.getElementById('shopping-list').innerHTML = data.shops.map(s => `
                <div class="card" style="display:flex; justify-content:space-between; align-items:center;">
                    <div><strong>${s.name}</strong> <span class="tag tag-pink">${s.area}</span><div style="font-size:11px; color:#64748b;">${s.type}</div></div>
                    <a href="https://map.naver.com/v5/search/${encodeURIComponent(s.name)}" target="_blank" class="link-btn">地圖</a>
                </div>
            `).join('');

            // 記帳
            let tot = 0;
            document.getElementById('expense-list').innerHTML = data.expenses.length ? data.expenses.map(e => {
                const aud = e.amt / data.rate; tot += aud;
                return `<div style="display:flex; justify-content:space-between; padding:4px 0; border-bottom:1px solid #eee;"><span>${e.item}</span><strong>$${aud.toFixed(2)} AUD</strong></div>`;
            }).join('') : '<div style="color:#94a3b8; text-align:center;">尚無記帳</div>';
            document.getElementById('total-aud').innerText = `$${tot.toFixed(2)}`;
            document.getElementById('per-aud').innerText = `$${(tot/4).toFixed(2)}`;

            // 打包
            document.getElementById('packing-list').innerHTML = data.packing.map(p => `
                <label style="display:block; padding:4px 0; font-size:13px;"><input type="checkbox"> ${p}</label>
            `).join('');
        }

        function tab(t) {
            ['itinerary','dining','shopping','expenses','packing'].forEach(id => {
                document.getElementById('sec-' + id).classList.add('hidden');
                document.getElementById('t-' + id).classList.remove('active');
            });
            document.getElementById('sec-' + t).classList.remove('hidden');
            document.getElementById('t-' + t).classList.add('active');
        }

        function changeDay(i) { data.activeDay = i; render(); }
        function updateRate() { data.rate = parseFloat(document.getElementById('ex-rate').value) || 900; render(); }
        function addDining() { const name = prompt('餐廳名稱:'); if(name) { data.dining.push({name, area:'首爾', type:'美食'}); render(); } }
        function addShop() { const name = prompt('店家名稱:'); if(name) { data.shops.push({name, area:'首爾', type:'購物'}); render(); } }
        function addExpense() {
            const item = prompt('消費項目:'); if(!item) return;
            const amt = parseFloat(prompt('金額 (KRW):')); if(amt) { data.expenses.push({item, amt}); render(); }
        }
        function addPacking() { const p = prompt('物品名稱:'); if(p) { data.packing.push(p); render(); } }

        // 時鐘
        setInterval(() => {
            const now = new Date();
            document.getElementById('clock').innerText = now.toLocaleTimeString("zh-TW", { timeZone: "Asia/Seoul", hour12: false });
        }, 1000);

        // 啟動首頁渲染
        render();
    </script>
</body>
</html>
