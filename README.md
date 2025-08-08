Quantum MVP Technical Specs Outline
1. Overview
Quantum MVP is a decentralized lottery on the Solana blockchain, designed for hourly draws with a Dynamic Odds system driven by Q-Score. This outline defines the minimal viable product (MVP) to establish core functionality and scalability.
2. Q-Score Mechanics

Standard Ticket ($1 USDC):
Cost: 1 USDC per ticket.
Effect: +0.1 Q-Score (fixed-point, 1.0 = 1000) on loss.
Win: Resets Q-Score to 1.0.


Streak: Increases by 1 per consecutive draw, resets to 0 on miss or win.
MVP Scope: Excludes Quantum Ticket, Cosmic Surge, and seasonal resets for simplicity.

3. Smart Contract

Blockchain: Solana for low fees and high throughput.
Instructions:
initialize: Sets up GameConfig with draw interval and initial Q-Score pool.
buy_ticket: Updates PlayerState with Q-Score and streak, adds USDC to PrizePot.
execute_draw: Uses Address Lookup Tables (ALTs) to process players, selects winner via Commit-Reveal, distributes 92% of PrizePot to winner, 8% to treasury.


Scalability: ALTs enable handling up to 256 players per draw for the MVP.
Randomness: Commit-Reveal scheme with a crank-submitted secret (future integration with Bitcoin blockhash TBD).

4. Data Structures
struct PlayerState {
    player_wallet: Pubkey, // Player's Solana address
    q_score: u64,         // Fixed-point, e.g., 1.0 = 1000
    streak: u32,          // Consecutive draws
}
struct GameConfig {
    draw_interval: i64,      // 3600 seconds (1 hour)
    total_q_score_in_pool: u64, // Sum of all players' Q-Scores
}
struct PrizePot {
    draw_id: u64,          // Unique draw identifier
    usdc_amount: u64,      // Total USDC collected
}

5. Crank Service

Role: Manages ALTs and submits V0 transactions for execute_draw.
MVP Implementation: Local Node.js script to populate ALTs and trigger draws.
Future: Decentralized keepers with $QTNM incentives.

6. Next Steps

Implement smart contract in Rust/Anchor.
Develop React frontend for ticket purchases and Q-Score display.
Test on Solana Devnet with simulated players.
