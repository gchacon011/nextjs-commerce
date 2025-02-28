# Himera Project
Description - A DEX integrated with an AI-powered trading agent. The trading agent (mammoth bot) analyzes market data, detects arbitrage opportunities, and executes trades automatically when the users want the agent to proceed. Users can assign their agents specific strategies, configure risk levels, and allocate funds depending on the parameters that the Users input. The trading agent (Mammoth Bot) dynamically adapts to real-time market conditions, optimizing trading performance to better provide insight to the end User.
## Table of Contents 
Frontend Architecture
Core Features
Backend Architecture
Smart Contract and Deployment on ABC Testnet
End to End

### Frontend Architecture

The architecture of the Himera Project Frontend:
   ◦ Next.js / React+ Tailwind CSS for a responsive UI 
   ◦ Features:
       • Token swap interface. 
       • Pool stats and liquidity management dashboard. 
       • Alagentrecommendations display (e.g., "Swap TokenA for TokenB"). 
       • Automation setup (user-defined triggers for swaps).
Description of capabilities of the frontend:  
        *Frontend** User interface can:  
                  - Enable wallet connection for secure interactions. 
                  - Display real-time DEX statistics, including =  Pool reserves / Token prices / Historical trade data. 
                  - Allow users to configure their AI agent:  Choosing trading strategies and setting risk levels to allocate funds. 
                  - Provide a dashboard to: - Monitor agent activity/  View trading performance (PnL, trade logs)/  Pause the agent and switch to manual trading.

### Core Features
        Core Features of this DEX 
                • Build abasic DEX 
                      ◦ Users can swap tokens directly on the platform using the Automated Market Maker (AMM) 
                      ◦ Token pairs are managed inliquidity pools 
                • AlAgentintegration 
                      ◦ Al analyzes market conditions and poolreserves to identify undervalued tokens. 
                      ◦ Automates swaps based on predefined strategies. 
                • Liquidity Pool Management 
                        ◦ Users can povide liquidity and earn trading fees 
                        ◦ Liquidity providers receive LP tokens for their share in the pool
                • Real-time Pool Stats 
                        ◦ Displaying key metrics (TVL, swap volume, token price etc) 
                • Automated Alerts and actions 
                      ◦ Agent triggers trades when specific conditions are met

### Backend Architecture
   The architecture of the Himera Project Backend:
   ◦ Node.js + expressjs 
   ◦ Features 
            • Processes Al agent's recommendations andinteracts withthe blockchain. 
            • Routes user requests to blockchain and automation layers. 
            • Manages user profiles, trade history, and APl integrations.
   The architecture of the AlAgent 
    ◦ Uses ElizaOS (TypeScript-based) or Python-based Al system. 
    ◦ Features 
             • Analyzes pool reserves, token prices, and trading volume. 
             • Identifies arbitrage opportunities and undervalued tokens. 
             • Suggests trades or executes them automatically via predefined rules. 
             •  Monitors market trends and liquidity conditions.

   The Blockchain architecture : 
     ◦ ABC Public Testnet 
      ◦ Features 
               • Smart contracts for: 
                     • Automated Market Maker (AMM) logic for token swaps. 
                     • Liquidity pools to handle reserves and fees. 
                     • Reward distribution for liquidity providers.

### Smart Contract and Deployment on ABC Testnet
         - Build smart contracts to:     
         - Develop a **minimal DEX** with:         
         - Single liquidity pool functionality for token pairs.         
         - Token swapping using the constant product formula (`x * y = k`).         
         - Events for `addLiquidity`, `removeLiquidity`, and `swap` for tracking activity.    
         - Implement mechanisms for AI agent-triggered trade execution.     
         - Store on-chain data for AI analysis, such as liquidity reserves and trade history. 
         - Deploy the following components on the ABC testnet:     
         - ERC20 token contracts for mock tokens (`TokenA` and `TokenB`).     
         - A lightweight AMM contract managing token swaps and liquidity. 
         
### Integration with Gelato Relay  
         - Enable gasless transactions with AI agents using Gelato Relay


### HIMERA DEX SMART CONTRACTS SYSTEM DESIGN:

Smart Contracts needed:
Token A: $CRASH, ERC20 Contract.
Token B: $BURN, ERC20 Contract.
Supply: 100.000 tokens each one.

AMM Contracts:
IMPORTANT, MUST HAVE: Token Swaps IN THE POOLS will always be done with this constant product formula: (X x Y = K).
EVERY swap in the pools should take a 0.3% fee, that would be kept in that pool, and added to its token reserve.

Pool 1 Contract:
This contract will store:
Token pair CRASH/BURN.
LP_1: Token issued to Liquidity Providers (ERC20 token that can be included inside the pool contract), to represent their part of the shares.
Reserve: This contract will store at the beginning a reserve of 2000 / 1000 (CRASH/BURN) tokens
Pool 2 Contract:
This contract will store:
Token pair CRASH/BURN.
LP_2: Token issued to Liquidity Providers (ERC20 token that can be included inside the pool contract), to represent their part of the shares.
Reserve: This contract will store at the beginning a reserve of 1000 / 5000 (CRASH/BURN) tokens
The different reserves in each pool creates a price difference, that will let the AI agent detect and execute the Arbitrage trading for the user, this is:
it swaps token A for token B in Pool 2, after this it swaps token B for token A in Pool 1, and finally it swaps part of token A for token B in Pool 2, depositing
then both tokens in the right ratio and getting LP_2 tokens issued to the user address.
And of course, the reverse operation for token B to get LP_1 tokens.

THIS TRADE STRATEGY COULD BE implemented "as is" (3 swaps and a deposit) OR as a Flash Loan, whatever you prefer to do.
If we use the Flash Loan, the user would not need to have bought token A or B beforehand.

Sales Contract:
Users could buy CRASH or BURN tokens in this contract.
Contract reserves: 10000 (5000 of each token)
Fixed price: 1 ABC Testnet token => 10 tokens (CRASH or BURN, same price for both).
ALL the testnet tokens spent by users will be sent to the farming contract, to get funds to distribute rewards.
This way, we can simulate users with real money buying tokens to get into the DEX and be able to trade, swap, participate in the pools,
claim back rewards and cashing out Testnet tokens as if there were real money.

Farming Contract:
We still have to think about this, because one mandatory requirement here is to use Gelato Web3 Functions to distribute rewards, but we could include a function
in the contract that can be called from the outside (public view) and when this function is called (every hour, for example), then rewards are calculated and the related Testnet
tokens are distributed among two maps of LP_1 and LP_2 token holders addresses.
This contract will hold the ABC Testnet tokens paid by users, calculate and distribute the rewards for Liquidity Providers.

##End-to-End Workflow 

Liquidity Provision
      ◦ User provide liquidity to token paids (tokenA/ tokenB),liquidity is added to the pool, and LP tokens are issues . 
Pool Stats Analysis 
      ◦ Agent monitors the pool reserves, swap volumes and token price fluctuations 
Recommendations 
      ◦ Suggestions for optimal trades or liquidity actions "TokenA is undervalued, consider swapping for TokenB . 
Trading: 
      ◦ Users initiate swaps based on agent insights 
Automation 
      ◦ Users can set conditions for automated swaps

