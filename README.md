# boundless

**在WSL中安装Boundless的步骤：**

1. **操作系统要求：**
    - 确保您的WSL发行版是 **Ubuntu 22.04 LTS**。如果不是，建议安装或更新到此版本。
2. **克隆Boundless仓库：**
    - 在您的WSL终端中执行以下命令：
        
        https://github.com/boundless-xyz/boundless/releases
        
        ```jsx
        git clone https://github.com/boundless-xyz/boundless
        cd boundless
        git checkout release-0.12
        ```
        
        ```jsx
        wget https://github.com/boundless-xyz/boundless/archive/refs/tags/v0.12.0.tar.gz
        tar -xzf v0.12.0.tar.gz
        cd boundless-0.12.0
        ```
        
3. **安装依赖：**
    - Boundless需要Docker Compose和Docker Nvidia支持。官方提供了一个便捷的设置脚本。
    - 在`boundless/`目录下运行：
        
        ```jsx
        sudo ./scripts/setup.sh
        ```
        
    - **重要提示：** 此脚本会安装Docker和Docker Nvidia。Docker Nvidia的安装过程需要启用NVIDIA的实验性软件包。
4. **设置环境变量：**
    - 您需要设置两个环境变量：`PRIVATE_KEY` 和 `RPC_URL`。
    - `PRIVATE_KEY` 是您将用于在市场上代表您的prover的钱包的私钥，请确保该钱包有资金。
    - `RPC_URL` 推荐使用Alchemy的Base Mainnet端点。
    - 您可以在每次会话中设置它们：
        
        ```jsx
        export PRIVATE_KEY="你的私钥"
        export RPC_URL="https://base-mainnet.g.alchemy.com/v2/{你的ALCHEMY_APP_ID}"
        ```
        
    - 或者，更推荐的方式是复制模板文件并编辑 `.env.broker` 文件：
        
        ```jsx
        cp .env.broker-template .env.broker
        ```
        
        然后使用文本编辑器（如 `nano` 或 `vim`）打开 `.env.broker` 文件并填入您的私钥和RPC URL。
        
5. **运行测试证明（可选但推荐）：**
    - 安装 `bento_cli`：
        
        ```jsx
        cargo install --locked --git https://github.com/risc0/risc0 bento-client --branch release-2.1 --bin bento_cli
        ```
        
    - 启动bento（不带broker）：
        
        ```jsx
        just bento
        ```
        
    - 在一个新的终端或会话中运行测试证明：
        
        ```jsx
        RUST_LOG=info bento_cli -c 32
        ```
        
    - 您可以使用 `just bento logs` 检查日志。
6. **安装Boundless CLI：**
    - 在运行broker之前，您需要安装Boundless CLI：
        
        ```jsx
        cargo install --locked boundless-cli
        ```
        
7. **存入质押金（Deposit Stake）：**
    - 如果您想参与Boundless市场，需要存入USDC作为质押金。
    - **注意：** 如果在Base Sepolia测试网上需要测试网USDC，请使用Circle Testnet Faucet。
    - 使用以下命令存入质押金（例如10个USDC）：
        
        ```jsx
        boundless account deposit-stake 10
        ```
        
8. **启动Broker：**
    - 这将同时运行bento和broker，即完整的证明堆栈：
        
        ```jsx
        just broker
        ```
        
    - 如果您使用了 `.env.broker` 文件，请这样启动：Bash
        
        ```jsx
        just broker up ./.env.broker
        ```
        
    - 使用 `just broker logs` 检查日志。
9. **停止Broker：**
    - 要停止broker：Bash
        
        ```jsx
        just broker down
        ```
        
    - 如果要移除所有卷和数据：Bash
        
        ```jsx
        just broker clean
        ```
