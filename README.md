# iartest STM32H533 (IAR + VS Code)

STM32CubeMX로 생성된 NUCLEO-H533RE 예제 프로젝트를 IAR Embedded Workbench와 VS Code에서 동시에 쾌적하게 다룰 수 있도록 구성한 저장소입니다.  
VS Code의 IntelliSense가 `compile_commands.json`을 그대로 활용하고, IAR 플러그인을 통해 빌드/디버깅이 가능하도록 `.vscode` 설정을 포함해 올려 두었습니다.

## 준비물
- **IAR Embedded Workbench for ARM 9.70.1** 이상 (경로 기본값: `C:\iar\ewarm-9.70.1`)
- **Visual Studio Code** (테스트 버전: 1.87 이상)
- VS Code 확장:
  - `C/C++` (ms-vscode.cpptools)
  - `C/C++ Extension Pack`
  - `C/C++ Themes`
  - `Codex – OpenAI’s coding agent`
  - `Embedded Tools`
  - `IAR Build`
  - `IAR C-SPY Debug`
  - `IAR Tools Extension Pack`
- (선택) STM32CubeMX 6.x – `iartest.ioc` 재생성용

## 폴더 구조
- `Core/` – STM32 HAL 애플리케이션 코드
- `Drivers/` – HAL/BSP/CMSIS 라이브러리
- `EWARM/` – IAR 프로젝트(`Project.eww`, `iartest.ewp`) 및 `compile_commands.json`
- `.vscode/` – VS Code용 설정 (`settings.json`, `iar-vsc.json`, `c_cpp_properties.json`)
- `iartest.ioc` – CubeMX 설정 파일

## VS Code에서 사용하기
1. 저장소를 클론하고 VS Code로 루트 폴더를 엽니다.
2. 확장 프로그램(`IAR for VS Code`, `C/C++`)을 설치합니다.
3. `.vscode/settings.json:6`에 지정된 경로(`C_Cpp.default.compileCommands`)가 `EWARM/iartest/compile_commands.json`을 가리키는지 확인합니다.  
   IAR에서 **Project → Options → C/C++ Compiler → Output → Generate debug information for C-SPY**가 켜져 있으면 `compile_commands.json`이 자동으로 갱신됩니다.
4. 명령 팔레트에서 `IAR: Open Workspace`를 실행하면 `.vscode/iar-vsc.json:4`에 정의된 IAR 워크벤치(`Project.eww`)가 열리고, 기본 구성(`iartest`)이 선택됩니다.
5. IAR 명령 팔레트 (`Ctrl+Shift+P → IAR: Build` / `IAR: Debug`)를 이용해 바로 빌드하거나 디버깅할 수 있습니다.

### IntelliSense가 정상 동작하지 않을 때
- IAR 설치 경로가 다른 경우 `.vscode/c_cpp_properties.json`에서 `C:/iar/ewarm-9.70.1` 부분을 실제 경로로 수정합니다.
- 캐시를 초기화하려면 VS Code에서 `C/C++: Reset IntelliSense Database` 명령을 실행하고 다시 열어 보세요.

## CubeMX 재생성 팁
`iartest.ioc`를 열어 설정을 바꾼 뒤 STM32CubeMX에서 코드를 다시 생성하면 `Core/`, `Drivers/`, `EWARM/`가 갱신됩니다.  
VS Code 설정 파일은 자동으로 덮어쓰지 않으니 변경 후에도 그대로 유지됩니다. 단, CubeMX가 생성한 새 파일이 `compile_commands.json`에 반영되도록 한 번 이상 IAR에서 빌드해 주세요.

## 참고 사항
- `.vscode/iar-vsc.json:5`의 `workbench.path`는 IAR 설치 위치에 맞게 업데이트할 수 있습니다.
- `build/`, `cache/`, `tmp/` 폴더는 로컬 빌드 산출물을 위해 사용하며 Git에 포함되지 않습니다.
- 프로젝트는 기본적으로 HAL 초기 설정만 포함되어 있으므로 사용자 코드는 `/* USER CODE BEGIN */` 블록 내부에 작성하면 CubeMX 재생성 시에도 안전하게 유지됩니다.
