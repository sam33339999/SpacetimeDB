<p align="center">
    <a href="https://spacetimedb.com#gh-dark-mode-only" target="_blank">
	<img width="320" src="./images/dark/logo.svg" alt="SpacetimeDB Logo">
    </a>
    <a href="https://spacetimedb.com#gh-light-mode-only" target="_blank">
	<img width="320" src="./images/light/logo.svg" alt="SpacetimeDB Logo">
    </a>
</p>
<p align="center">
    <a href="https://spacetimedb.com#gh-dark-mode-only" target="_blank">
        <img width="250" src="./images/dark/logo-text.svg" alt="SpacetimeDB">
    </a>
    <a href="https://spacetimedb.com#gh-light-mode-only" target="_blank">
        <img width="250" src="./images/light/logo-text.svg" alt="SpacetimeDB">
    </a>
    <h3 align="center">
        以光速進行開發。
    </h3>
</p>
<p align="center">
    <a href="https://github.com/clockworklabs/spacetimedb"><img src="https://img.shields.io/github/v/release/clockworklabs/spacetimedb?color=%23ff00a0&include_prereleases&label=version&sort=semver&style=flat-square"></a>
    &nbsp;
    <a href="https://github.com/clockworklabs/spacetimedb"><img src="https://img.shields.io/badge/built_with-Rust-dca282.svg?style=flat-square"></a>
    &nbsp;
	<a href="https://github.com/clockworklabs/spacetimedb/actions"><img src="https://img.shields.io/github/actions/workflow/status/clockworklabs/spacetimedb/ci.yml?style=flat-square&branch=master"></a>
    &nbsp;
    <a href="https://status.spacetimedb.com"><img src="https://img.shields.io/uptimerobot/ratio/7/m784409192-e472ca350bb615372ededed7?label=cloud%20uptime&style=flat-square"></a>
    &nbsp;
    <a href="https://hub.docker.com/r/clockworklabs/spacetimedb"><img src="https://img.shields.io/docker/pulls/clockworklabs/spacetimedb?style=flat-square"></a>
    &nbsp;
    <a href="https://github.com/clockworklabs/spacetimedb/blob/master/LICENSE.txt"><img src="https://img.shields.io/badge/license-BSL_1.1-00bfff.svg?style=flat-square"></a>
</p>
<p align="center">
    <a href="https://crates.io/crates/spacetimedb"><img src="https://img.shields.io/crates/d/spacetimedb?color=e45928&label=Rust%20Crate&style=flat-square"></a>
    &nbsp;
    <a href="https://www.nuget.org/packages/SpacetimeDB.Runtime"><img src="https://img.shields.io/nuget/dt/spacetimedb.runtime?color=0b6cff&label=NuGet%20Package&style=flat-square"></a>
</p>
<p align="center">
    <a href="https://discord.gg/spacetimedb"><img src="https://img.shields.io/discord/1037340874172014652?label=discord&style=flat-square&color=5a66f6"></a>
    &nbsp;
    <a href="https://twitter.com/spacetime_db"><img src="https://img.shields.io/badge/twitter-Follow_us-1d9bf0.svg?style=flat-square"></a>
    &nbsp;
    <a href="https://clockworklabs.io/join"><img src="https://img.shields.io/badge/careers-Join_us-86f7b7.svg?style=flat-square"></a>
    &nbsp;
    <a href="https://www.linkedin.com/company/clockworklabs/"><img src="https://img.shields.io/badge/linkedin-Connect_with_us-0a66c2.svg?style=flat-square"></a>
</p>

<p align="center">
    <a href="https://discord.gg/spacetimedb"><img height="25" src="./images/social/discord.svg" alt="Discord"></a>
    &nbsp;
    <a href="https://twitter.com/spacetime_db"><img height="25" src="./images/social/twitter.svg" alt="Twitter"></a>
    &nbsp;
    <a href="https://github.com/clockworklabs/spacetimedb"><img height="25" src="./images/social/github.svg" alt="GitHub"></a>
    &nbsp;
    <a href="https://twitch.tv/SpacetimeDB"><img height="25" src="./images/social/twitch.svg" alt="Twitch"></a>
    &nbsp;
    <a href="https://youtube.com/@SpacetimeDB"><img height="25" src="./images/social/youtube.svg" alt="YouTube"></a>
    &nbsp;
    <a href="https://www.linkedin.com/company/clockwork-labs/"><img height="25" src="./images/social/linkedin.svg" alt="LinkedIn"></a>
    &nbsp;
    <a href="https://stackoverflow.com/questions/tagged/spacetimedb"><img height="25" src="./images/social/stackoverflow.svg" alt="StackOverflow"></a>
</p>

<br>

> 📖 **其他語言版本：** [English (英文)](./README.md)

## 什麼是 [SpacetimeDB](https://spacetimedb.com)？

你可以把 SpacetimeDB 想像成資料庫與伺服器的結合體。

它是一個關聯式資料庫系統，讓你能夠透過稱為「模組（modules）」的進階預存程序，將你的應用程式邏輯直接上傳到資料庫中。

傳統架構中，你需要部署一個位於客戶端與資料庫之間的 Web 或遊戲伺服器。而使用 SpacetimeDB，你的客戶端可以直接連接到資料庫，並在資料庫內部執行應用程式邏輯。你可以把所有的權限與授權邏輯直接寫在模組中，就像寫在普通伺服器上一樣。

這意味著你可以用單一語言（Rust）撰寫整個應用程式，並部署為單一二進位檔。告別微服務、容器、Kubernetes、Docker、虛擬機器、DevOps、基礎設施、維運和伺服器。

<figure>
    <img src="./images/basic-architecture-diagram.png" alt="SpacetimeDB 架構" style="width:100%">
    <figcaption align="center">
        <p align="center"><b>SpacetimeDB 應用程式架構</b><br /><sup><sub>（白色元素由 SpacetimeDB 提供）</sub></sup></p>
    </figcaption>
</figure>

這個概念其實與智能合約類似，但 SpacetimeDB 是一個資料庫，與區塊鏈無關，且速度比任何智能合約系統快幾個數量級。

事實上，我們的 MMORPG 遊戲 [BitCraft Online](https://bitcraftonline.com) 的整個後端就只是一個 SpacetimeDB 模組。我們沒有執行任何其他伺服器或服務，這意味著遊戲中的一切——所有聊天訊息、物品、資源、地形，甚至玩家位置——都由資料庫儲存和處理，並即時同步到所有客戶端。

SpacetimeDB 針對最大速度和最低延遲進行優化，而非批次處理或 OLAP 工作負載。它專為即時應用程式設計，例如遊戲、聊天和協作工具。

這種速度和低延遲是透過將所有應用程式狀態保存在記憶體中來實現的，同時透過預寫日誌（WAL）持久化資料，用於恢復應用程式狀態。

## 安裝

你可以透過 `spacetime` CLI 工具將 SpacetimeDB 作為獨立資料庫伺服器執行。
以下列出各支援平台的安裝說明。
同樣的安裝說明也可在我們的網站 https://spacetimedb.com/install 找到。

#### 在 macOS 上安裝

在 macOS 上安裝只需執行我們的安裝腳本。之後你可以使用 spacetime 指令來管理版本。

```bash
curl -sSf https://install.spacetimedb.com | sh
```

#### 在 Linux 上安裝

在 Linux 上安裝只需執行我們的安裝腳本。之後你可以使用 spacetime 指令來管理版本。

```bash
curl -sSf https://install.spacetimedb.com | sh
```

#### 在 Windows 上安裝

在 Windows 上安裝只需將以下程式碼片段貼入 PowerShell。如果你想使用 WSL，請遵循 Linux 安裝說明。

```ps1
iwr https://windows.spacetimedb.com -useb | iex
```

#### 從原始碼安裝

關於從原始碼安裝的注意事項：除非 `master` 分支中有尚未發布的功能，否則我們建議使用官方安裝說明，而不要從原始碼安裝。

##### macOS + Linux

在 macOS + Linux 上安裝相當簡單。首先，我們需要建置所需的所有二進位檔：

```bash
# 安裝 rustup，如果你已安裝 cargo 和 wasm32-unknown-unknown 目標，可跳過此步驟。
curl https://sh.rustup.rs -sSf | sh
# 複製 SpacetimeDB
git clone https://github.com/clockworklabs/SpacetimeDB
# 建置並安裝 CLI
cd SpacetimeDB
cargo build --locked --release -p spacetimedb-standalone -p spacetimedb-update -p spacetimedb-cli

# 建立目錄
mkdir -p ~/.local/bin
export STDB_VERSION="$(./target/release/spacetimedb-cli --version | sed -n 's/.*spacetimedb tool version \([0-9.]*\);.*/\1/p')"
mkdir -p ~/.local/share/spacetime/bin/$STDB_VERSION

# 安裝更新二進位檔
cp target/release/spacetimedb-update ~/.local/bin/spacetime
cp target/release/spacetimedb-cli ~/.local/share/spacetime/bin/$STDB_VERSION
cp target/release/spacetimedb-standalone ~/.local/share/spacetime/bin/$STDB_VERSION
```

如果你尚未將 ~/.local/bin 加入路徑，現在需要進行此操作。

```
# 請將以下行加入你的 shell 設定檔，並開啟新的 shell 工作階段：
export PATH="$HOME/.local/bin:$PATH"

```

然後設定你的 SpacetimeDB 版本：
```

# 然後，在新的 shell 中，設定目前版本：
spacetime version use $STDB_VERSION

# 如果 STDB_VERSION 已不再設定，可使用以下指令列出版本：
spacetime version list
```

你可以透過 `spacetime --version` 驗證是否已安裝正確的版本。

##### Windows

在 Windows 上建置稍微複雜一些。你需要一個與大多數 Windows 終端機預裝版本不同的 Perl 版本。我們建議使用 [Strawberry Perl](https://strawberryperl.com/)。你可能還需要存取 `openssl` 二進位檔，它實際上預先安裝在 [Git for Windows](https://git-scm.com/downloads/win) 中。此外，你需要為 Windows 安裝 [rustup](https://rustup.rs/)。

在 Git for Windows shell 中，你應該會看到類似這樣的輸出：
```
$ which perl
/c/Strawberry/perl/bin/perl
$ which openssl
/mingw64/bin/openssl
$ which cargo 
/c/Users/<user>/.cargo/bin/cargo
```

如果看起來正確，你就可以繼續了！

```powershell
# 複製 SpacetimeDB
git clone https://github.com/clockworklabs/SpacetimeDB

# 建置並安裝 CLI
cd SpacetimeDB
cargo build --locked --release -p spacetimedb-standalone -p spacetimedb-update -p spacetimedb-cli

# 建立目錄
$stdbDir = "$HOME\AppData\Local\SpacetimeDB"
$stdbVersion = & ".\target\release\spacetimedb-cli" --version | Select-String -Pattern 'spacetimedb tool version ([0-9.]+);' | ForEach-Object { $_.Matches.Groups[1].Value }
New-Item -ItemType Directory -Path "$stdbDir\bin\$stdbVersion" -Force | Out-Null

# 安裝更新二進位檔
Copy-Item "target\release\spacetimedb-update.exe" "$stdbDir\spacetime.exe"
Copy-Item "target\release\spacetimedb-cli.exe" "$stdbDir\bin\$stdbVersion\"
Copy-Item "target\release\spacetimedb-standalone.exe" "$stdbDir\bin\$stdbVersion\"

```

現在將我們剛建立的目錄加入你的路徑。我們建議將其加入系統路徑，這樣所有應用程式（包括 Unity3D）都可以使用它。完成後，請重新啟動你的 shell！

```
%USERPROFILE%\AppData\Local\SpacetimeDB
```

然後，開啟新的 shell 並使用已安裝的 SpacetimeDB 版本：
```
spacetime version use $stdbVersion

# 如果 stdbVersion 已不再設定，可使用以下指令列出版本：
spacetime version list
```

你可以透過 `spacetime --version` 驗證是否已安裝正確的版本。

如果你使用 Git for Windows，可以改為遵循以下說明：

```bash
# 複製 SpacetimeDB
git clone https://github.com/clockworklabs/SpacetimeDB
# 建置並安裝 CLI
cd SpacetimeDB
# 建置 CLI 二進位檔 - 在 Windows 上需要一段時間，可以去喝杯咖啡 :)
cargo build --locked --release -p spacetimedb-standalone -p spacetimedb-update -p spacetimedb-cli

# 建立目錄
export STDB_VERSION="$(./target/release/spacetimedb-cli --version | sed -n 's/.*spacetimedb tool version \([0-9.]*\);.*/\1/p')"
mkdir -p ~/AppData/Local/SpacetimeDB/bin/$STDB_VERSION

# 安裝更新二進位檔
cp target/release/spacetimedb-update ~/AppData/Local/SpacetimeDB/spacetime
cp target/release/spacetimedb-cli ~/AppData/Local/SpacetimeDB/bin/$STDB_VERSION
cp target/release/spacetimedb-standalone ~/AppData/Local/SpacetimeDB/bin/$STDB_VERSION

# 現在將我們剛建立的目錄加入你的路徑。我們建議將其加入系統路徑，這樣所有應用程式（包括 Unity3D）都可以使用它。完成後，請重新啟動你的 shell！
# %USERPROFILE%\AppData\Local\SpacetimeDB

# 設定目前版本
spacetime version use $STDB_VERSION
```

你可以透過 `spacetime --version` 驗證是否已安裝正確的版本。

#### 使用 Docker 執行

如果你偏好在容器中執行 Spacetime，可以使用以下指令啟動一個新實例。

```bash
docker run --rm --pull always -p 3000:3000 clockworklabs/spacetime start
```

## 文件

如需了解更多關於 SpacetimeDB 的資訊、入門指南、遊戲開發指南和參考資料，請參閱我們的[文件](https://spacetimedb.com/docs)。

## 快速入門

我們為每種支援的語言準備了多個入門指南，幫助你盡快上手 SpacetimeDB。你可以在我們的[文件頁面](https://spacetimedb.com/docs)找到這些指南。

總結來說，開始使用 SpacetimeDB 只需 4 個步驟：

1. 安裝 `spacetime` CLI 工具。
2. 使用 `spacetime start` 啟動 SpacetimeDB 獨立節點。
3. 以支援的模組語言之一撰寫並上傳模組。
4. 使用客戶端函式庫之一連接到資料庫。

以下是支援語言的摘要，並附有每種語言的入門指南連結。

## 語言支援

你可以用多種流行語言撰寫 SpacetimeDB 模組，未來還會支援更多語言！

#### 伺服器端函式庫

- [Rust](https://spacetimedb.com/docs/modules/rust/quickstart)
- [C#](https://spacetimedb.com/docs/modules/c-sharp/quickstart)

#### 客戶端函式庫

- [Rust](https://spacetimedb.com/docs/sdks/rust/quickstart)
- [C#](https://spacetimedb.com/docs/sdks/c-sharp/quickstart)
- [Typescript](https://spacetimedb.com/docs/sdks/typescript/quickstart)

## 授權條款

SpacetimeDB 採用 BSL 1.1 授權條款。這不是開源或自由軟體授權，但幾年後會轉換為帶有連結例外的 AGPL v3.0 授權。

請注意，AGPL v3.0 通常不包含連結例外。我們為 SpacetimeDB 的 AGPL 授權添加了自訂連結例外。我們選擇自由軟體授權的動機是確保對 SpacetimeDB 的貢獻能回饋給社群。我們明確表示不希望強迫與 SpacetimeDB 連結的使用者開放其自己的程式碼，因此需要加入連結例外。
