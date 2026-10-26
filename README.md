# YanDrop

QR Code 配對 + WebRTC DataChannel 點對點檔案傳輸，適用於桌面與手機瀏覽器。

## 功能
- 使用 `qrcode.react` 產生 Offer / Answer QR Code。
- 使用 `html5-qrcode` 透過相機掃描配對 QR Code。
- 使用瀏覽器原生 WebRTC API 建立點對點連線並傳送檔案。
- 檔案以 16 KiB 區塊傳送並顯示進度。
- 靜態前端可透過 GitHub Pages 託管；檔案內容不會上傳到網站伺服器。

## 使用方式
1. 裝置 A 選擇「我要傳送檔案」並建立 Offer QR Code。
2. 裝置 B 選擇「我要接收檔案」，掃描 A 的 Offer QR Code。
3. B 顯示 Answer QR Code；A 掃描 B 的 Answer。
4. 等待 WebRTC DataChannel 連線成功。
5. A 選擇檔案並傳送；B 收到後下載。

相機存取通常要求 HTTPS。GitHub Pages 提供 HTTPS。

## WebRTC API 說明

### `RTCPeerConnection`
代表本機與遠端裝置之間的 WebRTC 連線。它建立連線並協商 ICE 網路路徑。

### `createOffer()` / `createAnswer()`
Offer 是發起端提出的連線描述；Answer 是接收端的回覆。YanDrop 將 SDP 描述放入 QR Code，讓兩台裝置交換配對資訊。

### `setLocalDescription()` / `setRemoteDescription()`
前者設定本機 Offer 或 Answer；後者設定掃描 QR Code 得到的遠端描述。

### ICE、STUN 與 TURN
ICE 尋找可用的網路路徑。QR Code 是帶外 signaling（信令交換）方式，不會取代 ICE。專案使用公開 STUN 服務 `stun:stun.l.google.com:19302`。某些 NAT、防火牆或受限制網路需要 TURN 中繼伺服器。

### `RTCDataChannel`
在 `RTCPeerConnection` 上建立雙向資料通道。檔案以中繼資料和二進位區塊傳送，接收端將區塊組成 Blob 供下載。

## 開發
```bash
npm install
npm run dev
npm run build
```

## GitHub Pages
將變更推送至 `main` 後，GitHub Actions 會建置並部署 `dist/`。若 Pages 尚未啟用，請到 **Settings → Pages → Build and deployment → Source** 選擇 **GitHub Actions**。

## 隱私與安全
檔案內容不會由 YanDrop 上傳到 GitHub Pages。配對 QR Code 包含連線資訊，請只與信任的裝置交換。某些網路可能無法建立直接連線；目前未內建 TURN 服務。

## License
MIT
