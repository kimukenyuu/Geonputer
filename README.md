# 💻 Geonputer

A comprehensive computer system simulator built from scratch using Java. 
This project integrates knowledge of **Compiler Theory, Computer Architecture, and Operating System Theory** to compile and execute programs written in a custom assembly-like language, **GeonSsembly**.

📖 **Language Documentation:** [GeonSsembly Language Syntax & Examples](https://github.com/DryRains/Geonputer-Documents)

---

<br/><br/>
## ✨ Key Features

* **Custom Compiler Pipeline**
   * Implements a custom Lexer and Parser to extract tokens and construct a Parse Tree via a Symbol Table, finally generating 16-bit Hexadecimal machine code.
* **RISC-based CPU Architecture Simulation**
   * Simulates a CPU with an Instruction Cycle (Fetch → Decode → Execute), internal Registers (PC, AC, SP, MAR, MBR, IR), and a simplified RISC instruction set.
* **OS Loader & Process Management**
   * Acts as an OS that loads executable files into Memory, allocates appropriate page and segment sizes (Data, Code, Stack, Heap Segments), and mounts them as independent Processes.
* **Context Switching & Interrupt Handling (Multi-threading)**
   * **Non-Blocking Mode:** Utilizes Java multi-threading to separate the CPU instruction cycle from user I/O inputs. Users can trigger an interrupt (e.g., press 's') to perform Context Switching—saving current PCB state, flushing the I/O buffer, and swapping processes dynamically.
   * **Blocking Mode:** Restricts user interrupts for pure, high-speed program execution.
* **Memory-Mapped I/O**
   * Communication between the CPU and I/O devices is seamlessly handled through Memory-Mapped I/O architecture.

---

<br/><br/>
## 🏗️ System Architecture & Directory Structure

The project is heavily decoupled into three main conceptual pillars: Compiler, OS, and Hardware System.

```text
Geonputer-main/
│
├── src/
│   ├── Compiler/             # Compiler Theory Implementation
│   │   ├── Lexer.java        # Lexical analysis and tokenization
│   │   ├── Parser.java       # Syntax analysis and AST construction
│   │   ├── CodeGenerator.java# Translates parsed statements into Hex machine code
│   │   └── SymbolTable.java  # Manages variables, labels, and memory mapping
│   │
│   ├── OS/                   # Operating System Theory Implementation
│   │   ├── Main.java         # Bootstrapper and main CLI interface
│   │   ├── Loader.java       # Allocates memory pages and loads Processes
│   │   ├── Linker.java       # Saves compiled object code to the disk
│   │   └── Process.java      # Process Control Block (PCB) state representation
│   │
│   └── System/               # Computer Architecture Implementation
│       ├── CPU.java          # Simulates ALU, Registers, and Instruction Cycles
│       ├── Memory.java       # Main memory containing Segments and IO Buffers
│       └── IODevice.java     # Handles user input and interrupt signals
│
├── Disk_SourceCode/          # Directory containing raw GeonSsembly source files
└── Disk_Program/             # Directory containing compiled executable hex files

```

## 🔄 Core Execution Flow

### 1. Compilation & Linking (Select `1` in CLI)

* The user selects a source code file from `Disk_SourceCode`.
* `Lexer` & `Parser` evaluate the source code, leveraging the `SymbolTable` to resolve labels and variables into structural sentences.
* `CodeGenerator` translates these sentences into 16-hex machine code.
* `Linker` writes the generated binary/hex string as an executable file into the `Disk_Program` folder.

### 2. Loading & Execution (Select `2` in CLI)

* The user selects a compiled program from `Disk_Program`.
* `Loader` analyzes the Data and Code segment sizes, allocates adequate Memory Pages, and maps the layout into a `Process` object.
* The `CPU` begins the continuous **Fetch-Decode-Execute** cycle, reading the Code Segment line-by-line.

### 3. Context Switching (During Execution)

* In **Non-Blocking Mode**, if the user inputs an 's' interrupt, the `CPU` halts its current cycle.
* The `CPU` saves the current state (PC, AC, SP, Index) into the active `Process` object.
* A new program is selected, and its previous state is restored from Memory (or loaded fresh), demonstrating a complete OS-level Context Switch.

## ⚙️ Dependencies

* **Java SE (JDK 17+)**: Core runtime environment.
* No external libraries or databases are required. The entire architecture is built natively using Java standard libraries.

## 🚀 Getting Started

### Prerequisites

* Java Development Kit (JDK) 17 or higher installed.

### Running the Application

1. Clone or extract the repository.
2. Open the project in your preferred IDE (Eclipse/IntelliJ) or use the command line.
3. Run `src/OS/Main.java`.
4. Follow the interactive CLI prompts:
* Press `1` to Compile & Link a `.txt` source file.
* Press `2` to Load & Run a compiled program.


5. **Note on Non-Blocking Mode:** Due to Java's multi-threading standard I/O scanner behavior, when the system requests a standard input during execution, you may need to type your input twice.

<br><br><br><br><br><br>
---
### コンパイラ論、コンピュータ構造論、オペレーティングシステム(OS)論を学習し、その知識を活用して独自のプログラミング言語で作成されたプログラムを実行できるコンピュータシステムを実装したプロジェクト
#### CPU Architecture : RISCベース、I/O Device - CPU通信方式 : Memory Mapped I/O
**Source Code : GeonSsembly Language**（アセンブリ言語をベースに作成した言語）。該当言語の文法定義と、その言語で作成したソースコードの例は[こちら](https://github.com/DryRains/Geonputer-Documents)を参照

プログラムの流れは以下の通りである。
1. プログラム実行後、1番を押してDisk_SourceCode内からコンパイルするソースコードを選択する。
2. Lexer & Parserがソースコードからトークンを抽出し、SymbolTableを通じてParse Tree Nodeに追加しながら文の構造を作成する。
3. Code Generatorがこれらの文を16進数の機械語に翻訳する。
4. その後、Linkerはこの変換された機械語を受け取り、Disk_Programフォルダに実行可能なファイルとして保存する。
5. プログラムを再実行し、2番を押して実行するファイルを選択する。
6. Loaderは選択されたファイルをメモリ上の適切なページ＆各セグメントサイズに割り当ててプロセスの形でロードし、CPUがInstruction Cycleを回しながら該当プロセスのCode Segmentを1行ずつ読み込んで解釈および実行する。
<br><br>

*5番でNon-Blocking ModeとBlocking Modeの2つの方式のうち1つを選択して実行できる。<br><br>
**Non-Blocking Mode** : CPUがInstruction Cycleを実行している間、ユーザーはそれとは別にキーボード入力によってI/O Interruptを発生させることができるモードである。<br>
「s」を押すと、メモリに割り当てられているI/O Bufferにこの入力が蓄積され、CPUは1サイクルを終えた後にI/O Bufferに割り込み(Interrupt)が入っているかをチェックする。この場合、「s」というユーザーの割り込みはContext Switchingを行うという命令としてマッピングされているため、CPUはこの割り込みを確認すると「Context Switching」を実行する。<br>
Context Switchingが実行されると、上記の5番と同様に実行するプログラムを選択するウィンドウが表示され、現在実行中だったプログラムの情報を一時保存＆I/O Bufferをflushする。もし選択したファイルが以前に実行中だったプロセスであれば、一時保存された情報を取り出して以前進行中だった作業を続けて実行し、
既存のメモリにロードされていないプロセスであれば、上記の6番と同様に適切なメモリ空間が割り当てられ、メモリに新しくロードされる。この過程を複数回実行してN個のプログラムをメモリにロードし、Context Switchingを行ってプロセスを切り替えることができる。<br>
（このモードはJavaのマルチスレッドを利用して実装されており、CPUクラスのInstruction CycleとIODeviceクラスのユーザー入力を受け取ってI/O Bufferに送るメソッドが別々に動作する。そのため、システムがユーザーに入力値を要求する際、Javaの入出力メソッドを2つのスレッドで同時に使用することになるため、入力値を2回入力しなければならない状況が発生するので注意 -> 単に入力する値を同じように2回入力すればよい。）<br><br>
**Blocking Mode** : ユーザーのInterruptを制限するモード。システムがユーザーに入力を要求した時のみ、ユーザーは入力を行うことができる。コンテキストスイッチング機能を使用せず、単にプログラムの動作だけを素早く確認したい場合は、このモードで実行すればよい。*

<br><br>
---
### 컴파일러론, 컴퓨터 구조론, 운영체제론을 학습하고 이 지식을 이용해 나만의 프로그래밍 언어로 작성된 프로그램을 실행할 수 있는 컴퓨터 시스템을 구현한 프로젝트
#### CPU Architecture : RISC 기반, I/O Device - CPU 통신 방식 : Memory Mapped I/O
**Source Code : GeonSsembly Language**(어셈블리어를 기반으로 만든 언어). 해당 언어의 문법 정의와 해당 언어로 만든 소스코드 예시는 [여기](https://github.com/DryRains/Geonputer-Documents)를 참고

프로그램의 흐름은 아래와 같다.
1. 프로그램 실행 후 1번을 눌러 Disk_SourceCode 내에 컴파일할 소스코드 선택
2. Lexer & Parser가 소스코드로부터 토큰들을 뜯어내고 SymbolTable을 통해 Parse Tree Node에 추가해가며 문장 구조를 만들어낸다.
3. Code Generator가 이 문장들을 16진수의 기계어로 번역한다.
4. 이후 Linker는 이 변환된 기계어를 받아 Disk_Program 폴더에 실행 가능한 파일로 저장한다.
5. 프로그램을 다시 실행 후 2번을 눌러 실행할 파일을 선택
6. Loader는 선택한 파일을 메모리에 적당한 페이지 & 각 세그먼트 사이즈를 할당하여 프로세스의 형태로 올리게 되고 CPU가 instruction Cycle을 돌면서 해당 프로세스의 Code Segment를 한줄씩 읽어 해석 및 실행한다.
<br><br>

*5번에서 Non-Blocking Mode와 Blocking Mode 두가지 방식중 하나를 선택해서 실행할 수 있다.<br><br>
**Non-Blocking Mode** : CPU가 Insturction Cycle을 수행하는 동안 사용자는 이와 별개로 키보드 입력을 통해 I/O Inturrupt를 발생시킬 수 있는 모드이다.<br>
's'를 누르면 메모리에 붙어있는 I/O Buffer에 이 입력이 쌓이게 되고 CPU는 한 사이클을 마치고 I/O Buffer에 인터럽트가 들어와 있는지 체킹한다. 이 경우 's' 라는 사용자의 인터럽트는 Context Switching을 하라는 명령으로 맵핑되어 있기에 CPU는 이 인터럽트를 확인하면 'Context Switching'을 수행한다.<br>
Context Switching이 수행되면 위의 5번과 동일하게 실행할 프로그램을 선택하는 창이 뜨고, 현재 실행중이던 프로그램의 정보들을 임시 저장 & I/O Buffer를 flush하고 만일 내가 선택한 파일이 이전에 실행중이던 프로세스면 임시 저장된 정보들을 가져와 이전에 진행중이던 작업을 이어서 수행, 
기존에 메모리에 올려져 있지 않은 프로세스면 위의 6번과 같이 적당한 메모리 공간이 할당되어 메모리에 새로 올라오게 된다. 이 과정을 여러변 수행하여 N개의 프로그램을 메모리에 올리고 Context Switching 하여 프로세스들을 전환할 수 있다.<br>
(이 모드는 자바의 멀티 스레드를 이용해 구현하여 CPU 클래스의 Intruction Cycle과 IO Device 클래스의 사용자의 입력을 받아 I/O Buffer로 보내는 메소드가 별개로 작동하는데, 시스템이 사용자에게 입력값을 요청할 때 자바의 입출력 메소드를 두 스레드에서 동시에 사용하게 되기 때문에 입력값을 두번 입력해야 하는 상황이 나타나니 참고 -> 그냥 입력할 값 똑같이 두번 입력해주면 된다.)<br><br>
**Bloking Mode** : 사용자의 Inturrupt를 제한하는 모드, 오로지 시스템이 사용자에게 입력을 요청할 때만 사용자는 입력을 할 수 있다. 컨텍스트 스위칭 기능을 사용하지 않고 단순히 프로그램의 작동만 빠르게 확인하려면 이 모드로 실행하면 된다.*
