# Only when user says "use oracle": Copy the bundle and paste into ChatGPT
# Include all files relevant to the debug session (error locations, stack traces, recently edited files, related components/utils)
# Use multiple --file flags or comma-separated patterns. Example:
npx @steipete/oracle --render --copy -p "<debug message>" --file "src/path/to/file1.ts" --file "src/path/to/file2.ts" --file "src/**/*.test.ts"
# Or with globs: --file "src/**/*.ts,src/**/*.test.ts,src-tauri/**/*.rs"
