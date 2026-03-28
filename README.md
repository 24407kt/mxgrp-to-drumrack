[English](#english) | [日本語](#日本語)

---

# mxgrp-to-drumrack <a id="english"></a>

Maschine Expansion Kit (.mxgrp) to Ableton Live 12 Drum Rack (.adg) batch converter.

Converts all Native Instruments Maschine Expansion Pack drum kits into Ableton Live Drum Rack presets with browser previews.

## Features

- Batch converts all `.mxgrp` kits across all Expansions
- Generates Ableton Live 12 compatible `.adg` files (gzipped XML)
- Copies `.ogg` preview files for Ableton browser preview playback
- Incremental updates (only converts new/modified kits)
- Auto-detects Ableton template location
- No external dependencies (Python standard library only)

## Requirements

- Python 3.6+
- Ableton Live 12 with **Drum Essentials** Pack installed (free, used as template)
- Native Instruments Maschine Expansion Packs

## Usage

```bash
# Basic usage (auto-detect template, output to NI Library subfolder)
python mxgrp_to_drumrack.py "D:\Native Instruments Library"

# Specify output folder
python mxgrp_to_drumrack.py "D:\Native Instruments Library" "D:\NI Drum Racks"

# Force regenerate all
python mxgrp_to_drumrack.py "D:\Native Instruments Library" "D:\NI Drum Racks" --force

# Specify template manually
python mxgrp_to_drumrack.py "D:\Native Instruments Library" --template "path/to/Wrong Sided Kit.adg"
```

## Setup in Ableton

1. Run the script to generate `.adg` files
2. In Ableton Live, go to **Places** > **Add Folder**
3. Select the output folder (e.g. `D:\NI Drum Racks`)
4. Browse your NI Expansion kits as Drum Rack presets with preview

## How It Works

1. **Parse `.mxgrp`**: Extracts pad-to-sample mappings from Maschine's binary kit files
2. **Template-based generation**: Uses a real Ableton `.adg` file (Wrong Sided Kit from Drum Essentials) as a structural template
3. **String replacement**: Replaces sample paths in the template while preserving the exact XML structure Ableton expects
4. **WAV analysis**: Reads WAV headers to set correct sample boundaries per pad
5. **Preview**: Copies NI's existing `.ogg` preview files into Ableton's `Ableton Folder Info/Previews/` structure

## NI Library Structure

```
D:\Native Instruments Library\
  Amplified Funk\
    Groups\Kits\
      3onIt_Kit.mxgrp
      .previews\3onIt_Kit.mxgrp.ogg
    Samples\Drums\Kick\Kick 3onIt 1.wav
  Aquarius Earth\
    ...
```

## Output Structure

```
D:\NI Drum Racks\
  Amplified Funk\
    3onIt_Kit.adg
  Aquarius Earth\
    Baby Mayka Kit.adg
  Ableton Folder Info\
    Properties.cfg
    Previews\
      Amplified Funk\
        3onIt_Kit.adg.ogg
      Aquarius Earth\
        Baby Mayka Kit.adg.ogg
```

## Limitations

- Maximum 16 pads per kit (Ableton Drum Rack limit from template)
- Maschine effects (Filter, Reverb, Delay, EQ, Compressor) are not converted
- Requires Drum Essentials Pack as template source
- Synth-based pads (no .wav sample) are skipped

## Technical Notes

### .mxgrp Format
Binary file containing UTF-16LE encoded sample paths. The parser extracts paths by searching for `.wav` byte sequences and reading surrounding ASCII characters.

### .adg Format
Gzip-compressed XML following Ableton Live 12's `GroupDevicePreset` schema (`MajorVersion="5"`, `SchemaChangeCount="6"`). Key structure:
- `GroupDevicePreset` > `InstrumentGroupDevice` > `DrumGroupDevice` > `BranchPresets` > `DrumBranchPreset` (per pad)
- Each pad contains an `OriginalSimpler` device with `MultiSamplePart` referencing the WAV file

### Why Template-Based?
Direct XML generation fails because Ableton requires the exact structure of a real preset file, including hundreds of default parameters per pad (~1,500 lines each). String replacement on a known-good template preserves this structure perfectly.

---

# mxgrp-to-drumrack <a id="日本語"></a>

Maschine Expansion Kit (.mxgrp) を Ableton Live 12 Drum Rack (.adg) に一括変換するツールです。

Native Instruments の全 Maschine Expansion Pack のドラムキットを、ブラウザプレビュー付きの Ableton Live Drum Rack プリセットに変換します。

## 特徴

- 全 Expansion の `.mxgrp` キットを一括変換
- Ableton Live 12 互換の `.adg` ファイルを生成（gzip圧縮XML）
- `.ogg` プレビューファイルをコピーし、Abletonブラウザでのプレビュー再生に対応
- 差分更新（新規・更新分のみ変換、`--force` で全再生成）
- Abletonテンプレートの自動検出
- 外部ライブラリ不要（Python標準ライブラリのみ）

## 必要なもの

- Python 3.6+
- Ableton Live 12 + **Drum Essentials** Pack（無料、テンプレートとして使用）
- Native Instruments Maschine Expansion Pack

## 使い方

```bash
# 基本（テンプレート自動検出、出力先はNI Library内サブフォルダ）
python mxgrp_to_drumrack.py "D:\Native Instruments Library"

# 出力先を指定
python mxgrp_to_drumrack.py "D:\Native Instruments Library" "D:\NI Drum Racks"

# 全て再生成
python mxgrp_to_drumrack.py "D:\Native Instruments Library" "D:\NI Drum Racks" --force

# テンプレートを手動指定
python mxgrp_to_drumrack.py "D:\Native Instruments Library" --template "path/to/Wrong Sided Kit.adg"
```

## Ableton での設定

1. スクリプトを実行して `.adg` ファイルを生成
2. Ableton Live で **場所** > **フォルダーを追加**
3. 出力フォルダ（例: `D:\NI Drum Racks`）を選択
4. NI Expansion のキットが Drum Rack プリセットとしてプレビュー付きで表示される

## 制限事項

- 1キットあたり最大16パッド（テンプレートの制限）
- Maschine のエフェクト（Filter, Reverb, Delay, EQ, Compressor）は変換されない
- テンプレートとして Drum Essentials Pack が必要
- シンセパッド（.wav サンプルがないパッド）はスキップ

## Disclaimer

This tool is provided as-is, without warranty of any kind. Use at your own risk. This project is not affiliated with, endorsed by, or associated with Native Instruments or Ableton. The `.mxgrp` file format is reverse-engineered; future updates to Maschine or Ableton may affect compatibility. Users are responsible for ensuring their use complies with applicable software license agreements.

本ツールは現状有姿で提供され、いかなる保証もありません。ご利用は自己責任でお願いします。本プロジェクトは Native Instruments および Ableton とは一切関係ありません。`.mxgrp` ファイル形式はリバースエンジニアリングによるものであり、Maschine や Ableton のアップデートにより互換性が失われる可能性があります。ご利用にあたっては、関連するソフトウェアのライセンス規約への準拠をご自身でご確認ください。

## License

MIT

---

If you find this tool useful, consider [supporting the project](https://buymeacoffee.com/embercore.jp).
