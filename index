const cron = require('node-cron');

const TOKEN = '8203807001:AAHPrb-hyJ2KKQje0bGrsKDJo28kh-1WFgg';
const CHAT_ID = '6016528682';

if (!TOKEN || !CHAT_ID) {
  console.error('Missing TELEGRAM_BOT_TOKEN or TELEGRAM_CHAT_ID');
  process.exit(1);
}

async function getPublicIP() {
  const res = await fetch('https://api.ipify.org/?format=json', {
    headers: { 'User-Agent': 'ip-checker/1.0' },
  });

  if (!res.ok) {
    throw new Error(`ipify error: HTTP ${res.status}`);
  }

  const data = await res.json();
  return data.ip?.trim();
}

async function sendTelegramMessage(text) {
  const res = await fetch(
    `https://api.telegram.org/bot${TOKEN}/sendMessage`,
    {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        chat_id: CHAT_ID,
        text,
      }),
    }
  );

  if (!res.ok) {
    throw new Error(`Telegram error: HTTP ${res.status}`);
  }
}

async function runOnce() {
  try {
    const currentIP = await getPublicIP();
    if (!currentIP) throw new Error('Invalid IP response');

	const message = `✅ Current Public IP: ${currentIP}`;
	await sendTelegramMessage(message);

  } catch (err) {
    console.error('Job error:', err.message);
  }
}

// chạy ngay khi start
runOnce();

// chạy mỗi phút
cron.schedule('* * * * *', runOnce);

console.log('IP monitor started (check every minute)');
