# PyTorchのDocker環境例

B3向けDockerガイド。

## 想定環境
- Visual Studio Code
- Docker

## 簡単な解説
- 読めばわかる気がするのでなし。

Cudaのバージョンが高くてauroraでしか動かないので、それ以外の計算機ではimageのバージョンを下げる。
SSHを使用するために.ssh/をbind mountしているがimageを公開する場合は非推奨。ssh-agentを使用すれば安全らしい。
