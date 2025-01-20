import 'package:flutter/material.dart';

void main() {
  runApp(const CalculatorApp());
}

class CalculatorApp extends StatelessWidget {
  const CalculatorApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Calculadora',
      theme: ThemeData(
        primarySwatch: Colors.lightBlue,
        scaffoldBackgroundColor: Colors.white,
        textTheme: const TextTheme(
          bodyMedium: TextStyle(color: Colors.black),
        ),
      ),
      home: const CalculatorHome(),
    );
  }
}

class CalculatorHome extends StatefulWidget {
  const CalculatorHome({super.key});

  @override
  State<CalculatorHome> createState() => _CalculatorHomeState();
}

class _CalculatorHomeState extends State<CalculatorHome> {
  String _output = "0";
  String _operator = "";
  double? _firstOperand;
  String _input = "";

  void _buttonPressed(String buttonText) {
    setState(() {
      if (buttonText == "C") {
        _output = "0";
        _input = "";
        _operator = "";
        _firstOperand = null;
      } else if (buttonText == "+" || buttonText == "-" || buttonText == "x" || buttonText == "/") {
        if (_firstOperand == null) {
          _firstOperand = double.tryParse(_input);
        }
        _operator = buttonText;
        _input = "";
      } else if (buttonText == "=") {
        if (_firstOperand != null && _input.isNotEmpty) {
          double secondOperand = double.parse(_input);
          switch (_operator) {
            case "+":
              _output = (_firstOperand! + secondOperand).toString();
              break;
            case "-":
              _output = (_firstOperand! - secondOperand).toString();
              break;
            case "x":
              _output = (_firstOperand! * secondOperand).toString();
              break;
            case "/":
              _output = secondOperand != 0
                  ? (_firstOperand! / secondOperand).toString()
                  : "Erro";
              break;
          }
          _firstOperand = null;
          _operator = "";
          _input = "";
        }
      } else {
        _input += buttonText;
        _output = _input;
      }
    });
  }

  Widget _buildButton(String text, Color backgroundColor, {Color textColor = Colors.black}) {
    return Expanded(
      child: ElevatedButton(
        style: ElevatedButton.styleFrom(
          backgroundColor: backgroundColor,
          padding: const EdgeInsets.all(20),
          shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
        ),
        onPressed: () => _buttonPressed(text),
        child: Text(
          text,
          style: TextStyle(fontSize: 24, color: textColor, fontWeight: FontWeight.w600),
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Calculadora"),
        backgroundColor: Colors.lightBlueAccent,
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        children: [
          Container(
            padding: const EdgeInsets.all(16),
            alignment: Alignment.centerRight,
            child: Text(
              _output,
              style: const TextStyle(
                fontSize: 48,
                fontWeight: FontWeight.bold,
                color: Colors.black,
              ),
            ),
          ),
          const Divider(),
          Column(
            children: [
              Row(
                children: [
                  _buildButton("7", Colors.lightBlue[50]!),
                  _buildButton("8", Colors.lightBlue[50]!),
                  _buildButton("9", Colors.lightBlue[50]!),
                  _buildButton("/", Colors.lightBlue[200]!, textColor: Colors.white),
                ],
              ),
              Row(
                children: [
                  _buildButton("4", Colors.lightBlue[50]!),
                  _buildButton("5", Colors.lightBlue[50]!),
                  _buildButton("6", Colors.lightBlue[50]!),
                  _buildButton("x", Colors.lightBlue[200]!, textColor: Colors.white),
                ],
              ),
              Row(
                children: [
                  _buildButton("1", Colors.lightBlue[50]!),
                  _buildButton("2", Colors.lightBlue[50]!),
                  _buildButton("3", Colors.lightBlue[50]!),
                  _buildButton("-", Colors.lightBlue[200]!, textColor: Colors.white),
                ],
              ),
              Row(
                children: [
                  _buildButton("C", Colors.red[100]!, textColor: Colors.red[800]!),
                  _buildButton("0", Colors.lightBlue[50]!),
                  _buildButton("=", Colors.green[100]!, textColor: Colors.green[800]!),
                  _buildButon("+", Colors.lightBlue[200]!, textColor: Colors.white),
                ],
              ),
           
        ]
  }
}
