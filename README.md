# EODHCTSiqsxajs Unified OP 1.0

`EODHCTSiqsxajs-V3.0.json` is a single SillyTavern Chat Completion preset.

The default final reply is English narrative prose. Provider-returned reasoning is requested and retained separately from the story. No regex removes reasoning, and no completion sentence is needed to find the start of the story.


### Prompt build order

1. `eod_variable_initialization` resets every module variable and restores the medium pacing fallback.
2. Enabled functional setters populate only their assigned variables. Disabled setters leave their variables blank, so stale values from a previous preset or turn cannot silently reactivate them.
3. The selected pacing setter and stage limiter run before the dispatcher reads them.
4. The reasoning contract and main task establish channel and story responsibilities.
5. Writing guidelines consume persona-independent narrative rules, autonomy, relationship, reality, information, and ensemble modules.
6. Reference sections contain creative material, character settings, examples, and interaction history.
7. Style and output sections consume only the enabled modules assigned to them.
8. Operator definitions consume the scene matrix and final audit. The final action ends planning and requests story content.

Keep the initializer first and all setters before consumers when editing prompt order. `injection_position: 0` uses the listed relative order; `injection_order` numbers are not used as a substitute for setter/reader ordering.

### Story consistency rules

Current explicit user corrections and established world/character facts outrank new inventions. The archive persona supplies framing; it does not grant characters access to narrator-only information. Reader intent guides craft without deciding the user's actions or guaranteeing the protagonist's success. LORE may add compatible details without rewriting canon. ARC obeys pacing, relationship stages, physical costs, and user agency. REVIEW applies the original audit criteria within the same planning process.

### Additional OP-style options

| Identifier | Effect when enabled | Default |
| --- | --- | --- |
| `eod_status_optional` | Factual scene-status code block before the narrative; unknown values stay unknown | Off |
| `eod_length_optional` | Soft target of roughly 250 English words for main prose, subordinate to scene and pacing needs | Off |
| `eod_expressions_optional` | OP's expressions-to-avoid list | Off |
| `eod_custom_writing` | Editable writing-rule setter | Off / blank |
| `eod_custom_style` | Editable style-rule setter | Off / blank |

The optional length target is roughly 250 English words, a soft guideline rather than a requirement. Enabling the summary module requests a short factual summary of roughly 35 English words. The optional status bar uses English labels and “Unknown” for unspecified dates or weather. OP's unrelated style restrictions and status/summary defaults are not forced onto the merged story behavior.

## Import and use

1. This preset is designed exclusively for the DeepSeek V4-Pro and Flash models; performance with other models cannot be guaranteed.
3. Keep the reasoning/show-thoughts setting enabled if you want the client to request and retain provider-returned reasoning. Turning the preset flag off is not an equivalent UI-only operation for the inspected native DeepSeek backend.
4. In Prompt Manager, leave the architecture entries enabled. The default pace is medium. To select another persistent default, enable exactly one of the four `eod_pacing_*` selectors and disable the other three. With all four off, the initializer supplies medium as a fallback. If multiple selectors are enabled, the last enabled setter in prompt order wins; select one to avoid that ambiguity.
5. Enable any desired optional modules. To change the output language, edit `output_format` or disable it and request a language directly. Custom writing/style entries are blank templates: edit the value after the second `::` inside their `setvar` macro, then enable them.
6. If SillyTavern asks whether preset-scoped regex scripts may run, review and allow the scripts you intend to use. The reasoning-channel separation does not depend on regex permission. Enable the optional META wrapper separately in the preset's regex controls only if you want that framing.

All active architecture and functional prompt blocks use user-role task material. Standard markers supply the original source context. Provider role post-processing still belongs to the selected connection.

### Pacing and turn-level requests

For a persistent default, use the selector toggles described above. A user instruction such as “slow down this turn” may override narrative pacing for that turn through the dispatcher prompt. It does **not** execute a client macro or permanently change the stored selector configuration. The preset does not claim an automatic keyword router.

### Optional visible additions

When enabled, final content follows this order:

1. Factual status bar.
2. Main narrative.
3. `<Summary>...</Summary>`.
4. `<Unresolved>...</Unresolved>`.

Disabled additions are omitted. The summary display regex can render a summary as a collapsible details element; raw summary tags remain in stored content. These records must not contain planning, secret information, or a reasoning transcript. Continuation resumes existing content without restarting the status bar or replaying earlier prose.

### Regex behavior

- The optional META wrapper applies to user text **only in the prompt view**. It avoids wrapping an input already beginning with the exact wrapper prefix. Stored user messages remain intact.
- Table-edit and proxy/row-command cleanup applies to display and prompt views, for both user and assistant placement. The original stored text remains available to memory extensions and exports. These are cosmetic/context filters, not database executors.
- Summary rendering is display-only for assistant messages.