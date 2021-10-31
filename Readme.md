# board-game-rs

[![Crates.io](https://img.shields.io/crates/v/board-game)](https://crates.io/crates/board-game)
[![CI status](https://github.com/KarelPeeters/board-game-rs/actions/workflows/rust.yml/badge.svg)](https://github.com/KarelPeeters/board-game-rs/actions)

<!--
Everything within the cargo-sync-readme comments is autogenerated based on the crate-level docs in lib.rs.
DO NOT EDIT MANUALLY
-->

<!-- cargo-sync-readme start -->

A [Board](https://docs.rs/board-game/latest/board-game/board/trait.Board.html) abstraction for deterministic two player
games. This allows for code to be generic over the actual game, so it only needs to written once.

# Features

Currently, the implemented games are:
* [Super/Ultimate tic-tac-toe](https://en.wikipedia.org/wiki/Ultimate_tic-tac-toe)
  in the module [sttt](https://docs.rs/board-game/latest/board-game/games/sttt/).
* [Ataxx](https://en.wikipedia.org/wiki/Ataxx)
  in the module [ataxx](https://docs.rs/board-game/latest/board-game/games/ataxx/).
* Chess in the module [chess](https://docs.rs/board-game/latest/board-game/games/chess/), implemented as a simple wrapper around the [chess](https://crates.io/crates/chess) crate.

Notable things currently implemented in this crate that work for any [Board](https://docs.rs/board-game/latest/board-game/board/trait.Board.html):
* Game-playing algorithms, specifically:
  * [RandomBot](https://docs.rs/board-game/latest/board-game/ai/simple/struct.RandomBot.html), which simply picks a random move.
  * [RolloutBot](https://docs.rs/board-game/latest/board-game/ai/simple/struct.RolloutBot.html), which simulates a fixed number of random games for each possible move and picks the one with the best win probability.
  * [MinimaxBot](https://docs.rs/board-game/latest/board-game/ai/minimax/struct.MiniMaxBot.html), which picks the best move as evaluated by a customizable heuristic at a fixed depth. (implemented as alpha-beta negamax).
  * [MCTSBot](https://docs.rs/board-game/latest/board-game/ai/mcts/struct.MCTSBot.html), which picks the best move as found by [Monte Carlo Tree Search](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search).
* Random board generation functions, see [board_gen](https://docs.rs/board-game/latest/board-game/util/board_gen/).
* A bot vs bot game runner to compare playing strength, see [bot_game](https://docs.rs/board-game/latest/board-game/util/bot_game/).
* Simple game statistics (perft, random game length) which can be used to test [Board](https://docs.rs/board-game/latest/board-game/board/trait.Board.html) implementations.

# Examples

## List the available moves on a board and play a random one.

```rust

let mut board = AtaxxBoard::default();
println!("{}", board);

board.available_moves().for_each(|mv| {
println!("{:?}", mv)
});

let mv = board.random_available_move(&mut rng);
println!("Picked move {:?}", mv);
board.play(mv);
println!("{}", board);
```

## Get the best move according to MCTS

```rust

let board = AtaxxBoard::default();
println!("{}", board);

let mut bot = MCTSBot::new(1000, 2.0, thread_rng());
println!("{:?}", bot.select_move(&board))
```

<!-- cargo-sync-readme end -->
