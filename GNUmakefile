# パーストレース用のデバッグヘルパー (debug/parse-trace ブランチ専用)。
#
# GNU make は Makefile より先に GNUmakefile を読むため、ここに include する形で
# upstream の Makefile に触れずにターゲットを追加している。
#
# 使い方:
#   make trace                     # デフォルトサンプル (samples/f002.rb) のトレース
#   make trace F=samples/binary.rb # 対象ファイルを指定
#   make ast  F=samples/binary.rb  # AST のみ
#   make parse F=...               # トレース + AST
#   make lex  F=...                # トークン列
#   make samples                   # サンプル一覧
#
# ビルドはソース変更時のみ自動で走る (lib/prism/prism.bundle の依存解決)。
include Makefile

# 解析対象の Ruby ファイル。samples/ 配下に置く。
F ?= samples/f002.rb

# トレース入り拡張のビルド。src/include が bundle より新しいときだけ rake compile。
lib/prism/prism.bundle: $(SOURCES) $(HEADERS)
	rake compile

.PHONY: trace ast parse lex samples

# パース過程のトレースのみ (stderr 側)
trace: lib/prism/prism.bundle
	ruby -Ilib bin/prism parse $(F) 2>&1 1>/dev/null

# AST のみ (stdout 側)
ast: lib/prism/prism.bundle
	ruby -Ilib bin/prism parse $(F) 2>/dev/null

# トレース + AST
parse: lib/prism/prism.bundle
	ruby -Ilib bin/prism parse $(F)

# トークン列
lex: lib/prism/prism.bundle
	ruby -Ilib bin/prism lex $(F) 2>/dev/null

samples:
	@ls -1 samples/
