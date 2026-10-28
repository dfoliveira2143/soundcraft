# SoundCraft — funcionalidades e módulos

Última atualização: 2026-10-28.

## Fluxo principal

| Etapa | Entradas | Responsabilidade e dependências | Saídas |
|---|---|---|---|
| Abrir e editar | Áudio, MIDI, sessões e comandos de edição | [audio-io](../../crates/audio-io/), [midi](../../crates/midi/), [model](../../crates/model/), [engine](../../crates/engine/) | Sessão com trilhas, clips, roteamento e automação |
| Avaliar e produzir saída | Estado editado e parâmetros de saída | [dsp](../../crates/dsp/), [mix](../../crates/mix/), [playback](../../crates/playback/) | Áudio em tempo real e bounce offline |
| Acesso pelo usuário ou agente | Gestos, argumentos CLI ou chamadas estruturadas | [apps](../../apps/), [engine](../../crates/engine/) e interfaces descritas abaixo | Estado atualizado, resposta ou erro; comportamento detalhado nos contratos |

A tabela mostra o caminho conceitual, não uma garantia de suporte a todos os formatos. APIs, estruturas, unidades, efeitos colaterais e limites ficam na [arquitetura](../architecture.md), nos documentos de assunto e nos módulos indicados.

## Contratos e exemplos de uso

- [aux-routing-order.md](../aux-routing-order.md)
- [plugin-parity.md](../plugin-parity.md)
- [commit-multichannel.md](../commit-multichannel.md)
- [strip-mute-automation.md](../strip-mute-automation.md)
- [control-protocol.md](../control-protocol.md)
- [mcp.md](../mcp.md)

## Inventário de bibliotecas

Responsabilidades resumidas a partir dos manifestos e comentários de módulo do checkout. As dependências listadas são as internas diretas declaradas em `dependencies` (incluindo condicionais); não incluem dependências de teste/build nem expansão de features. O manifesto de cada linha é a fonte exata.

| Módulo / manifesto | Responsabilidade declarada | Dependências internas diretas |
|---|---|---|
| [au-host](../../crates/au-host/Cargo.toml) | SoundCraft Audio Unit (AUv2, macOS) plugin host: lists, loads and runs system-registered Audio Units behind the safe soundcraft-dsp Plugin trait (an isolated unsafe FFI crate) | `soundcraft-dsp` |
| [audio-io](../../crates/audio-io/Cargo.toml) | SoundCraft audio file I/O: WAV/BWF/RF64, AIFF/AIFF-C native readers and writers, FLAC encoder, symphonia-backed decode of compressed formats, waveform peaks | Nenhuma declarada nesta categoria |
| [automation](../../crates/automation/Cargo.toml) | SoundCraft agent control: MCP server (stdio JSON-RPC), headless engine backend and control-channel client | `soundcraft-engine`, `soundcraft-time` |
| [clap-host](../../crates/clap-host/Cargo.toml) | SoundCraft CLAP plugin host: scans, loads and runs third-party CLAP plugins behind the safe soundcraft-dsp Plugin trait (isolated plugin FFI; exceptions are listed in AGENTS.md) | `soundcraft-dsp` |
| [dsp](../../crates/dsp/Cargo.toml) | SoundCraft DSP: realtime plugins (EQ, dynamics, reverb, delay, modulation, instruments), meters, loudness, and offline AudioSuite-style processing | Nenhuma declarada nesta categoria |
| [engine](../../crates/engine/Cargo.toml) | SoundCraft command engine: commands, undo, editing operations, import/export, bounce, parity catalog | `soundcraft-au-host`, `soundcraft-audio-io`, `soundcraft-clap-host`, `soundcraft-dsp`, `soundcraft-midi`, `soundcraft-mix`, `soundcraft-model`, `soundcraft-time`, `soundcraft-video`, `soundcraft-vst3-host` |
| [midi](../../crates/midi/Cargo.toml) | SoundCraft MIDI: note/controller sequences, Standard MIDI File import/export, MIDI editing operations, GM names | Nenhuma declarada nesta categoria |
| [mix](../../crates/mix/Cargo.toml) | SoundCraft mix engine: renders a session's tracks, plugins, sends, busses and automation to audio | `soundcraft-au-host`, `soundcraft-audio-io`, `soundcraft-clap-host`, `soundcraft-dsp`, `soundcraft-midi`, `soundcraft-model`, `soundcraft-time`, `soundcraft-vst3-host` |
| [model](../../crates/model/Cargo.toml) | SoundCraft session document: tracks, playlists, clips, mixer, routing, automation, markers | `soundcraft-audio-io`, `soundcraft-midi`, `soundcraft-time` |
| [playback](../../crates/playback/Cargo.toml) | SoundCraft audio device I/O: realtime playback and recording through the mix engine | `soundcraft-audio-io`, `soundcraft-dsp`, `soundcraft-mix`, `soundcraft-model`, `soundcraft-time` |
| [time](../../crates/time/Cargo.toml) | SoundCraft timebases: samples, Bars/Beats, Min:Secs, Timecode, Feet+Frames, tempo and meter maps, grid | Nenhuma declarada nesta categoria |
| [ui-egui](../../crates/ui-egui/Cargo.toml) | SoundCraft's egui user interface: Edit and Mix windows, transport, dialogs, control channel | `soundcraft-au-host`, `soundcraft-audio-io`, `soundcraft-clap-host`, `soundcraft-dsp`, `soundcraft-engine`, `soundcraft-midi`, `soundcraft-mix`, `soundcraft-model`, `soundcraft-playback`, `soundcraft-time`, `soundcraft-video`, `soundcraft-vst3-host` |
| [video](../../crates/video/Cargo.toml) | SoundCraft video: MP4/MOV demux, H.264 / ProRes / Motion JPEG decoding to RGBA, timeline thumbnails | Nenhuma declarada nesta categoria |
| [vst3-host](../../crates/vst3-host/Cargo.toml) | SoundCraft VST3 plugin host: scans, loads and runs third-party VST3 plugins behind the safe soundcraft-dsp Plugin trait (an isolated unsafe FFI crate) | `soundcraft-dsp` |

Ao mudar uma entrada, saída ou dependência, atualize primeiro o contrato do módulo e depois esta navegação. [Voltar ao índice](../INDEX.md).
