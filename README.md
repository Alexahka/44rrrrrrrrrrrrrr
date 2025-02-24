import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Casino Game',
      theme: ThemeData.dark(), // Black background theme
      home: CasinoHomePage(),
    );
  }
}

class CasinoHomePage extends StatefulWidget {
  @override
  _CasinoHomePageState createState() => _CasinoHomePageState();
}

class _CasinoHomePageState extends State<CasinoHomePage> {
  int score = 0;

  void _increaseScore() {
    setState(() {
      score += 10; // Increment score when a button is clicked
    });
  }

  void _openMiniGame(int gameNumber) {
    Navigator.push(
      context,
      MaterialPageRoute(
        builder: (context) => MiniGamePage(gameNumber: gameNumber, onScoreUpdated: _increaseScore),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Casino Game')),
      backgroundColor: Colors.black,
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // Buttons with different colors
            Row(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                _buildCasinoButton('Game 1', Colors.red, 1),
                SizedBox(width: 20),
                _buildCasinoButton('Game 2', Colors.blue, 2),
                SizedBox(width: 20),
                _buildCasinoButton('Game 3', Colors.green, 3),
              ],
            ),
            SizedBox(height: 40),
            // Scoreboard at the bottom
            Text(
              'Score: $score',
              style: TextStyle(fontSize: 24, color: Colors.white),
            ),
          ],
        ),
      ),
    );
  }

  Widget _buildCasinoButton(String title, Color color, int gameNumber) {
    return ElevatedButton(
      onPressed: () => _openMiniGame(gameNumber),
      style: ElevatedButton.styleFrom(primary: color),
      child: Text(title, style: TextStyle(fontSize: 18)),
    );
  }
}

class MiniGamePage extends StatefulWidget {
  final int gameNumber;
  final Function onScoreUpdated;

  MiniGamePage({required this.gameNumber, required this.onScoreUpdated});

  @override
  _MiniGamePageState createState() => _MiniGamePageState();
}

class _MiniGamePageState extends State<MiniGamePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Mini Game ${widget.gameNumber}')),
      backgroundColor: Colors.black,
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Text(
              'Mini Game ${widget.gameNumber} is here!',
              style: TextStyle(fontSize: 24, color: Colors.white),
            ),
            SizedBox(height: 40),
            ElevatedButton(
              onPressed: () {
                widget.onScoreUpdated(); // Update the score when the mini game is played
                Navigator.pop(context);
              },
              style: ElevatedButton.styleFrom(primary: Colors.purple),
              child: Text('Finish Game', style: TextStyle(fontSize: 18)),
            ),
          ],
        ),
      ),
    );
  }
}
