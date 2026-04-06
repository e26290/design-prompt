Do not create documentation, demo boards, preview matrices, helper
components, or decorative layouts. Do not ask for more design decisions
unless a required existing asset is missing and work cannot continue.

Create a dedicated page named:

Button

Create a local component set named:

Button

Style target:

Mature, scalable, design-system-quality button component suitable for
future React implementation. Prioritize semantic token usage, clean
properties, maintainable structure, and disciplined auto layout.

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

FIRST: INSPECT AND REUSE

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Before building, inspect existing:

\- local variables

\- local text styles

\- local components

Always reuse existing:

\- semantic variables

\- local text styles

\- local icon components

If a truly required token is missing, add only the minimum missing
token(s) using the existing naming architecture.

Do not create new text styles unless absolutely unavoidable.

Required existing components:

\- \`placeholder-icon\` for leading/trailing icons

\- \`loader-icon\` for loading

If either cannot be found, stop and ask me for the exact node link.

Do not redraw, recreate, or substitute them.

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

PROPERTIES

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Create these exact properties:

1\. appearance

\- primary

\- secondary

2\. tone

\- default

\- destructive

3\. state

\- default

\- hover

\- pressed

\- focused

\- disabled

4\. size

\- medium

5\. leadingIcon

\- boolean

6\. trailingIcon

\- boolean

7\. text

\- text property

Also use instance swap for icon instances where possible.

Rules:

\- destructive must be controlled only by \`tone\`

\- focused must remain a \`state\`, not a boolean

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

BUILD RULES

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\- Use auto layout throughout

\- Main outer button frame must hug contents by default

\- Button must stay single-line

\- Width must grow naturally with longer text

\- Use padding-based sizing, not fixed width

\- Use minimum heights per size

\- Keep naming and variant organization disciplined

\- Keep the page clean and production-oriented

Required structure:

\- outer button frame hugs contents by default

\- inner visual container fills outer container so manual resize works
properly

\- centered content row inside the visual container

Important:

If a button instance is manually widened, the inner visual container
must also expand correctly.

Focused, loading, and any internal visual wrapper must preserve correct
resize behavior too.

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

SIZING

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Sizes:

\- medium

Map size through:

\- horizontal padding

\- vertical padding

\- minimum height

\- text style

\- icon size

\- content gap

Text styles:

\- medium → body/sm

Icon sizes:

\- medium → 16

Use existing semantic dimension tokens wherever possible.

Radius:

\- use \`radius/control/sm\` consistently

\- do not vary radius by size

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

APPEARANCE

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

primary:

\- filled emphasis

secondary:

\- subtle neutral surface

Tone behavior:

\- \`default\` uses default semantic tokens

\- \`destructive\` remaps to danger/destructive semantic tokens while
preserving appearance structure

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

STATES

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

States:

\- default

\- hover

\- pressed

\- focused

\- disabled

Behavior:

\- default = base visual

\- hover = subtle interaction change

\- pressed = stronger active state

\- focused = focus ring + default visual

\- disabled = neutralized and non-interactive

Disabled rules:

\- use disabled semantic surface/text/icon/border tokens

\- no hover treatment

\- no pressed treatment

\- no focus ring

Focus rules:

\- use a reliable focus-ring implementation

\- a wrapper/focus host is allowed only if needed

\- keep the solution clean and resize-safe

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

ICONS

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Support:

\- optional leading icon

\- optional trailing icon

Rules:

\- use boolean visibility properties

\- use instance swap where possible

\- use existing \`placeholder-icon\`

\- icon color must use semantic icon variables

\- icon spacing must use tokens

\- do not create icon-only buttons in this task

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

TEXT

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Use the label as an editable text property.

Rules:

\- use existing local text styles

\- use sentence case

\- do not force uppercase

\- text color must use semantic text variables appropriate to
appearance, tone, and state

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

FINAL RESPONSE

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

After building, reply with a very short status only:

\- Button page created: yes/no

\- Button component set created: yes/no

\- \`placeholder-icon\` found: yes/no

\- \`loader-icon\` found: yes/no

\- missing supporting component stopped work: yes/no

\- missing tokens added: yes/no

\- anything not completed exactly: short note
