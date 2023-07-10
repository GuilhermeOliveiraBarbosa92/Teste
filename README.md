# Jogo
import 'package:flutter/material.dart';
import 'package:flame/game.dart';
import 'package:flame/flame.dart';
import 'package:flame/components.dart';
import 'package:flame/keyboard.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Flame.util.fullScreen();
  await Flame.util.setOrientation(DeviceOrientation.portraitUp);

  final game = CircleGame();
  runApp(game.widget);

  Flame.util.addGestureRecognizer(TapGestureRecognizer()
    ..onTapDown = game.onTapDown
    ..onTapUp = game.onTapUp);
}

class Circle extends PositionComponent {
  static const double radius = 50;
  static const double speed = 250;

  @override
  void render(Canvas canvas) {
    super.render(canvas);
    final paint = Paint()..color = const Color(0xFFFF0000);
    canvas.drawCircle(position, radius, paint);
  }

  @override
  void update(double dt) {
    super.update(dt);
    position.y += speed * dt;
  }
}

class CircleGame extends Game with KeyboardEvents {
  Circle circle;

  @override
  Future<void> onLoad() async {
    circle = Circle()
      ..anchor = Anchor.center
      ..x = size.x / 2
      ..y = 0;
    add(circle);
  }

  @override
  void render(Canvas canvas) {
    super.render(canvas);
  }

  @override
  void update(double dt) {
    super.update(dt);
    if (circle.y >= size.y - Circle.radius) {
      gameOver();
    }
  }

  void gameOver() {
    print('Game Over');
    // Reinicie o jogo ou faça outra ação quando o jogo terminar.
  }

  void onTapDown(TapDownDetails details) {
    if (circle.toRect().contains(details.localPosition)) {
      circle.remove();
      // Adicione sua lógica para aumentar a pontuação aqui.
    }
  }

  @override
  void onKeyEvent(RawKeyEvent event) {
    // Adicione qualquer lógica de controle de teclado, se necessário.
  }
}
