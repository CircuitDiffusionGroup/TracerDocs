# Tracer: A PCB Description Language

## Overview

Tracer is a concise, domain-specific language for describing printed circuit board (PCB) layouts. It uses a unique, minimalist syntax to define components, their connections, and pin assignments, enabling engineers and hobbyists to specify PCB designs efficiently. Tracer is particularly suited for rapid prototyping and clear, readable PCB descriptions.

## Features

- **Minimalist Syntax**: Define components and connections with a compact, function-like notation.
- **Hierarchical Design**: Support for modular blocks to reuse subcircuits.

## Getting Started

### Prerequisites

- Tracer compiler binary (download from official site - link TBD).
- A text editor (e.g., VS Code, preferably with the official VS code Tracer plugin, Sublime Text).

### Installation

1. Download the Tracer compiler binary for your platform (Windows/Linux/macOS) from official site (link TBD).
2. Extract the binary to a directory of your choice.
3. Add the binary to your system PATH or run it directly.

### Example 1: Gyro Array with STM32

This script defines a PCB with an STM32 microcontroller and four gyro sensors, each with a unique chip-select pin:

```
<STM32;
<imu;
<cap;

gyro(cs=cs) {
    imu(
        p1=!MISO, p2=!GND, p3=!GND, 
        p4=nc1, p5=!VCC, p6=!GND, p7=!GND, 
        p8=!VCC, p9=nc2, p10=!GND, p11=!GND, 
        p12=cs, p13=!SCLK, p14=!MOSI
    );
    cap(p1=!VCC, p2=!GND);
}

gyro(cs=PB9);
gyro(cs=PB8);
gyro(cs=PB7);
gyro(cs=PB6);

STM32(p40=VCC, p39=GND, p37=PB9, p36=PB8, p35=PB7, p34=PB6, p10=SCLK, p9=MISO, p8=MOSI);
```

Run the compiler:

```
tracer -t example1.t -o output.cds
```

This generates a graph file with the STM32 and gyro array intended for automatic PCB creation.

### Example 2: LED Circuit with Battery

This script defines a PCB with a battery-powered astable multivibrator:

```
<bat;
<led;
<R47K;
<R470;
<cap;
<npn;

ledRes(a=a, c=c) {
    led(a=a, c=r);
    R470(a=r, b=c);
}

ledVCC(c=c) {
    ledRes(a=!VCC, c=c);
}

bat(p=VCC, n=GND);

ledVCC(c=C1P);
ledVCC(c=C2P);

R47K(a=VCC, b=C1N);
R47K(a=VCC, b=C2N);

cap(p=C1P, n=C1N);
cap(p=C2P, n=C2N);

npn(c=C1P, b=C2N, e=GND);
npn(c=C2P, b=C1N, e=GND);
```

Run the compiler:

```
tracer -t example2.t -o output.cds
```

## Documentation

- Syntax Guide: Detailed explanation of Tracer's syntax and conventions.
- Examples: Additional Tracer scripts for common circuits.
- Tutorials: Step-by-step guides for designing PCBs with Tracer.

## Syntax Highlights

- **Component Declaration**: Use `<component;` to declare components (e.g., `<STM32;`, `<led;`).
- **Blocks**: Define reusable subcircuits with function-like syntax (e.g., `gyro(cs=cs) { ... }`).
- **Pin Assignment**: Use `pN=value` for pins, with `!` for global signals (e.g., `!VCC`).
- **Connections**: Implicitly connect components via shared signals (e.g., `C1P`, `VCC`).

## Notes

- The Tracer compiler is closed-source and distributed as a binary. Source code is not available at this moment.
- Check the official site (link TBD) for the latest binary releases and updates.

## Contributing

While the compiler is closed-source, we welcome feedback, bug reports, and example contributions:

- Submit issues or suggestions via GitHub Issues.
- Share your Tracer scripts in the examples directory via pull requests.

## License

Tracer documentation and examples are released under the MIT License. The Tracer compiler binary is subject to its own licensing terms (see official site - link TBD).

## Contact

- **Issues**: Report bugs or request features on GitHub Issues.
- **Community**: Join discussions on Discord (link TBD).
- **Email**: Contact us at your.email@example.com.

Start designing your PCBs with Tracer today!
