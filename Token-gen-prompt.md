This must be built as a production-ready foundation for:

1\. a very large Figma component kit

2\. light/dark theming

3\. radius mode switching

4\. future React token usage

Do not create component-specific tokens.

Do not simplify the system.

Do not skip scopes.

Do not leave typography partially connected.

Do not use raw values on text styles when variable binding is possible.

────────────────────────────────

PHASE 1 --- ASK ONLY THESE 4 QUESTIONS

────────────────────────────────

Before creating anything, ask the user these 4 questions only, in a
short and beginner-friendly way.

Question 1 --- Brand color

Ask:

"What should the main brand color be?

Examples:

\- #0C6FF9

\- blue

\- royal blue

If you only give a color name, choose a sensible accessible ramp around
it."

Question 2 --- Accent color (optional)

Ask:

"Do you want an optional accent color?

Examples:

\- #FFB700

\- yellow

\- orange

\- no

If you say no, keep the system without a strong accent ramp."

Question 3 --- Neutral color family

Ask:

"Which neutral family should power the UI?

Choose one:

\- gray

\- slate

\- zinc

\- stone"

Question 4 --- Heading and body font

Ask:

"What font should be used for headings and body text?

Examples:

\- Inter

\- Manrope

\- Satoshi

\- SF Pro

If the font is unavailable, fall back to Inter."

After the user answers, briefly restate the chosen setup in 4 short
bullet points, then immediately build the system.

Keep code font fixed as:

\- JetBrains Mono

────────────────────────────────

PHASE 2 --- FIXED SYSTEM ARCHITECTURE

────────────────────────────────

Everything below stays fixed except brand color, accent color, neutral
family, and heading/body font.

Create EXACTLY these local variable collections:

PRIMITIVE COLLECTIONS

\- primitives.color

\- primitives.dimension

\- primitives.radius

\- primitives.typography

\- primitives.number

\- primitives.shadow

SEMANTIC COLLECTIONS

\- semantic.color

\- semantic.dimension

\- semantic.radius

\- semantic.typography

\- semantic.elevation

\- semantic.number

Modes:

\- semantic.color → modes: light, dark

\- semantic.elevation → modes: light, dark

\- semantic.radius → modes: default, rounded, no-corner-radius

\- all primitive collections → single mode only

\- semantic.typography → single mode only

\- semantic.dimension → single mode only

\- semantic.number → single mode only

Use slash naming everywhere.

Semantic variables must alias primitive variables wherever appropriate.

Do not duplicate primitive values inside semantic collections unless
absolutely required by Figma limitations.

────────────────────────────────

PHASE 3 --- TOKEN MODEL TO CREATE

────────────────────────────────

A\) PRIMITIVES --- COLOR

Create color primitives with ramps for:

\- transparent

\- black

\- white

\- neutral family chosen by the user

\- brand

\- accent (only if user requested one)

\- success

\- warning

\- danger

\- info

Use practical product-design ramp system:

\- 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950

Suggested naming examples:

\- color/gray/50

\- color/gray/100

\- color/brand/500

\- color/accent/500

\- color/success/600

\- color/warning/500

\- color/danger/600

\- color/info/600

\- color/black

\- color/white

\- color/transparent

Build a sensible accessible brand ramp from the provided brand color.

Build a sensible accent ramp from the accent color if provided.

For neutral families, use the selected family name exactly in token
names.

B\) PRIMITIVES --- DIMENSION

Create a 4px-based dimension system with useful small and large steps
for product UI.

Use names like:

\- size/0 = 0

\- size/1 = 4

\- size/2 = 8

\- size/3 = 12

\- size/4 = 16

\- size/5 = 20

\- size/6 = 24

\- size/7 = 28

\- size/8 = 32

\- size/10 = 40

\- size/12 = 48

\- size/14 = 56

\- size/16 = 64

\- size/20 = 80

\- size/24 = 96

\- size/32 = 128

Also create useful stroke-width primitives here if needed, or in
primitives.number if your implementation is cleaner.

Keep naming clean and consistent.

C\) PRIMITIVES --- RADIUS

Create primitive radius values:

\- radius/xs = 4

\- radius/sm = 8

\- radius/md = 12

\- radius/lg = 16

\- radius/xl = 20

\- radius/2xl = 24

\- radius/3xl = 32

\- radius/pill = 9999

Also create rounded-mode primitive values:

\- radius-rounded/xs = 8

\- radius-rounded/sm = 12

\- radius-rounded/md = 16

\- radius-rounded/lg = 20

\- radius-rounded/xl = 24

\- radius-rounded/2xl = 32

\- radius-rounded/3xl = 40

\- radius-rounded/pill = 9999

Also create no-radius primitive values:

\- radius-none/xs = 0

\- radius-none/sm = 0

\- radius-none/md = 0

\- radius-none/lg = 0

\- radius-none/xl = 0

\- radius-none/2xl = 0

\- radius-none/3xl = 0

\- radius-none/pill = 4

D\) PRIMITIVES --- TYPOGRAPHY

Split primitive typography by property.

Create:

1\. font families

\- font-family/body = user-selected font

\- font-family/heading = user-selected font

\- font-family/code = JetBrains Mono

2\. font weights

Use NUMBER variables for fontWeight.

Create at least:

\- font-weight/regular = 400

\- font-weight/medium = 500

\- font-weight/semibold = 600

\- font-weight/bold = 700

3\. font sizes

Balanced SaaS app scale:

\- font-size/xs = 12

\- font-size/sm = 14

\- font-size/md = 16

\- font-size/lg = 18

\- font-size/xl = 20

\- font-size/2xl = 24

\- font-size/3xl = 30

\- font-size/4xl = 36

\- font-size/5xl = 48

4\. line heights

Create practical pixel values, for example:

\- line-height/xs = 16

\- line-height/sm = 20

\- line-height/md = 24

\- line-height/lg = 28

\- line-height/xl = 28

\- line-height/2xl = 32

\- line-height/3xl = 38

\- line-height/4xl = 44

\- line-height/5xl = 56

5\. letter spacing

Create number variables:

\- tracking/tighter

\- tracking/tight

\- tracking/normal

\- tracking/wide

Use practical values suitable for Figma text properties.

6\. paragraph spacing

Create:

\- paragraph-spacing/none = 0

\- paragraph-spacing/sm = 4

\- paragraph-spacing/md = 8

\- paragraph-spacing/lg = 12

7\. paragraph indent

Create:

\- paragraph-indent/none = 0

E\) PRIMITIVES --- NUMBER

Create reusable number primitives for:

\- opacity/0

\- opacity/5

\- opacity/10

\- opacity/20

\- opacity/30

\- opacity/40

\- opacity/50

\- opacity/60

\- opacity/70

\- opacity/80

\- opacity/90

\- opacity/100

Also include practical numbers for:

\- stroke-width/0

\- stroke-width/1

\- stroke-width/2

\- stroke-width/3

\- stroke-width/4

F\) PRIMITIVES --- SHADOW

Create shadow/elevation recipes for code handoff and semantic aliasing.

Use strings if necessary for shadow recipes.

Create at least:

\- shadow/none

\- shadow/sm

\- shadow/md

\- shadow/lg

\- shadow/xl

These should reflect realistic product UI shadows and allow a stronger
dark-mode treatment where needed downstream.

────────────────────────────────

PHASE 4 --- SEMANTIC TOKEN STRUCTURE

────────────────────────────────

A\) SEMANTIC.COLOR

Create a mature, non-component-specific semantic color system using
property-first naming.

Must include these families at minimum:

SURFACE

\- surface/bg/canvas

\- surface/bg/default

\- surface/bg/raised

\- surface/bg/overlay

\- surface/bg/sunken

\- surface/bg/brand

\- surface/bg/brand/hover

\- surface/bg/brand/pressed

\- surface/bg/brand/subtle

\- surface/bg/brand/subtle/hover

\- surface/bg/brand/subtle/pressed

\- surface/bg/accent

\- surface/bg/accent/subtle

\- surface/bg/success

\- surface/bg/success/subtle

\- surface/bg/warning

\- surface/bg/warning/subtle

\- surface/bg/danger

\- surface/bg/danger/subtle

\- surface/bg/info

\- surface/bg/info/subtle

\- surface/bg/selected

\- surface/bg/selected/hover

\- surface/bg/selected/pressed

\- surface/bg/disabled

TEXT

\- text/default

\- text/subtle

\- text/muted

\- text/inverse

\- text/brand

\- text/brand/inverse

\- text/accent

\- text/success

\- text/warning

\- text/danger

\- text/info

\- text/disabled

\- text/on-brand

\- text/on-accent

\- text/on-success

\- text/on-warning

\- text/on-danger

\- text/on-info

ICON

\- icon/default

\- icon/subtle

\- icon/muted

\- icon/inverse

\- icon/brand

\- icon/accent

\- icon/success

\- icon/warning

\- icon/danger

\- icon/info

\- icon/disabled

\- icon/on-brand

\- icon/on-accent

\- icon/on-success

\- icon/on-warning

\- icon/on-danger

\- icon/on-info

BORDER

\- border/default

\- border/subtle

\- border/strong

\- border/brand

\- border/accent

\- border/success

\- border/warning

\- border/danger

\- border/info

\- border/selected

\- border/focus

\- border/disabled

STATIC / UTILITY

\- background/neutral

\- background/inverse

\- overlay/default

\- overlay/strong

All semantic color tokens must alias appropriate primitive colors.

Create different aliases for light and dark modes.

Do not leave light and dark identical unless truly intentional.

B\) SEMANTIC.DIMENSION

Create semantic spacing tokens for layout use:

\- space/inset/none

\- space/inset/xs

\- space/inset/sm

\- space/inset/md

\- space/inset/lg

\- space/inset/xl

\- space/inset/2xl

\- space/stack/none

\- space/stack/xs

\- space/stack/sm

\- space/stack/md

\- space/stack/lg

\- space/stack/xl

\- space/stack/2xl

\- space/inline/none

\- space/inline/xs

\- space/inline/sm

\- space/inline/md

\- space/inline/lg

\- space/inline/xl

\- space/inline/2xl

\- space/section/sm

\- space/section/md

\- space/section/lg

\- space/section/xl

\- space/section/2xl

\- space/section/3xl

Alias these to the primitive size values.

C\) SEMANTIC.RADIUS

Create semantic radius tokens with 3 modes:

\- default

\- rounded

\- no-corner-radius

Tokens:

\- radius/control/xs

\- radius/control/sm

\- radius/control/md

\- radius/control/lg

\- radius/container/sm

\- radius/container/md

\- radius/container/lg

\- radius/container/xl

\- radius/dialog

\- radius/pill

Each mode must alias the corresponding primitive radius values.

D\) SEMANTIC.TYPOGRAPHY

Create semantic typography tokens by property, all with one single mode
only.

Create property tokens for these text styles:

\- display

\- heading/h1

\- heading/h2

\- heading/h3

\- heading/h4

\- title/lg

\- title/md

\- body/lg

\- body/md

\- body/sm

\- caption

\- code/md

\- code/sm

For each style, create semantic variables for:

\- font-family

\- font-weight

\- font-size

\- line-height

\- letter-spacing

\- paragraph-spacing

Naming pattern:

\- typography/display/font-family

\- typography/display/font-weight

\- typography/display/font-size

\- typography/display/line-height

\- typography/display/letter-spacing

\- typography/display/paragraph-spacing

And same for each style, for example:

\- typography/heading/h1/font-family

\- typography/heading/h1/font-weight

\- typography/heading/h1/font-size

\...etc

Rules:

\- headings use mixed hierarchy:

\- h1 = bold

\- h2, h3, h4 = semibold

\- body styles use medium

\- code styles use JetBrains Mono

\- no dedicated label styles

\- paragraph-spacing can be 0 for most UI text unless a style genuinely
needs it

Recommended style mapping:

\- display → heading family, 48 / 56, bold

\- heading/h1 → heading family, 36 / 44, bold

\- heading/h2 → heading family, 30 / 38, semibold

\- heading/h3 → heading family, 24 / 32, semibold

\- heading/h4 → heading family, 20 / 28, semibold

\- title/lg → heading family, 18 / 28, semibold

\- title/md → heading family, 16 / 24, semibold

\- body/lg → body family, 18 / 28, medium

\- body/md → body family, 16 / 24, medium

\- body/sm → body family, 14 / 20, medium

\- caption → body family, 12 / 16, medium

\- code/md → code family, 14 / 20, medium or regular depending font
rendering; use the closest appropriate token

\- code/sm → code family, 12 / 16, medium or regular depending font
rendering; use the closest appropriate token

All semantic typography variables must alias primitive typography
variables.

E\) SEMANTIC.ELEVATION

Create light/dark semantic elevation tokens:

\- elevation/surface/sunken

\- elevation/surface/default

\- elevation/surface/raised

\- elevation/surface/overlay

\- elevation/shadow/none

\- elevation/shadow/sm

\- elevation/shadow/md

\- elevation/shadow/lg

\- elevation/shadow/xl

Surface tokens should alias semantic/primitives color structure
appropriately.

Shadow tokens should alias shadow primitives where possible.

F\) SEMANTIC.NUMBER

Create semantic number tokens as aliases where useful, for example:

\- opacity/disabled

\- opacity/subtle

\- opacity/overlay

\- stroke-width/default

\- stroke-width/strong

\- stroke-width/focus

────────────────────────────────

PHASE 5 --- VARIABLE SCOPES

────────────────────────────────

Scope every variable to its relevant purpose only.

Do not leave variables on "all supported properties" unless there is a
strong reason.

Apply scopes like this:

COLOR SCOPES

1\. Primitive colors:

\- keep available only where sensible

\- general palette colors may be scoped broadly, but prefer discipline

2\. Semantic surface/background colors:

Scope to:

\- frame fill

\- shape fill

\- effects when relevant for overlays

3\. Semantic text colors:

Scope to:

\- text fill only

4\. Semantic icon/border colors:

\- icon colors → shape fill

\- border colors → stroke

\- focus/overlay colors can also include effects if relevant

NUMBER SCOPES

1\. semantic.dimension spacing tokens:

\- auto layout gap

\- auto layout padding

2\. semantic.radius and primitive radius:

\- corner radius only

3\. font-size variables:

\- font size only

4\. line-height variables:

\- line height only

5\. letter-spacing variables:

\- letter spacing only

6\. paragraph-spacing variables:

\- paragraph spacing only

7\. paragraph-indent variables:

\- paragraph indent only

8\. font-weight NUMBER variables:

\- font weight only

9\. opacity tokens:

\- layer opacity only

10\. stroke-width tokens:

\- stroke only

11\. any size tokens intended for width/height utilities:

\- width and height only

Only do this if those tokens are truly intended for size, not for
spacing.

STRING SCOPES

1\. font-family variables:

\- font family only

2\. any text-string utility variables:

\- text string only

Only create these if actually needed. Otherwise skip them.

3\. if MCP or API requires string fallback for font weight/style, use:

\- font weight or style only

Only do this as a fallback if direct number binding fails.

Important:

Be deliberate with scoping.

This is a mature token architecture, so scope should reduce noise and
prevent misuse.

────────────────────────────────

PHASE 6 --- CODE SYNTAX METADATA

────────────────────────────────

Where supported, set WEB code syntax metadata on variables for cleaner
Dev Mode / React handoff.

Use slash-based token names converted into CSS custom property style
where reasonable.

Example approach:

\- color/brand/500 → var(\--color-brand-500)

\- surface/bg/default → var(\--surface-bg-default)

\- typography/body/md/font-size → var(\--typography-body-md-font-size)

Apply code syntax consistently.

────────────────────────────────

PHASE 7 --- LOCAL TEXT STYLES

────────────────────────────────

Create local text styles for:

\- display

\- heading/h1

\- heading/h2

\- heading/h3

\- heading/h4

\- title/lg

\- title/md

\- body/lg

\- body/md

\- body/sm

\- caption

\- code/md

\- code/sm

Use these exact local style names:

\- display

\- heading/h1

\- heading/h2

\- heading/h3

\- heading/h4

\- title/lg

\- title/md

\- body/lg

\- body/md

\- body/sm

\- caption

\- code/md

\- code/sm

For each local text style:

1\. create the style

2\. bind each supported property to the matching semantic typography
variable:

\- font family

\- font weight

\- font size

\- line height

\- letter spacing

\- paragraph spacing

3\. do not manually type values where variable binding is available

4\. ensure the style is actually bound, not just visually matched

Important implementation detail:

Use the correct text-style variable binding API / MCP method.

Bind the style properties, not only the sample text node.

The goal is for the text style itself to stay token-driven.

If MCP fails to bind fontWeight as a number variable:

\- attempt the correct compatible alternative supported by the current
Figma API/MCP runtime

\- keep all other typography properties variable-bound

\- do not stop unless absolutely necessary

Load fonts before applying font-dependent text properties.

If the requested heading/body font is unavailable:

\- fall back to Inter for heading and body automatically

If JetBrains Mono is unavailable:

\- use the closest available monospace fallback, but prefer JetBrains
Mono first

────────────────────────────────

PHASE 8 --- TYPOGRAPHY PREVIEW FRAME

────────────────────────────────

Create a new page called \"Typography\" and inside it Create a small,
neat "Typography preview" frame.

Requirements:

\- title the frame: Typography preview

\- use semantic surface/text color tokens only

\- use auto layout strictly.

\- clean spacing using semantic dimension tokens

\- inside it, show each text style with:

\- style name

\- a sample sentence

\- Make sure each container width is set to hug contents.

Suggested sample sentence:

"The quick brown fox jumps over the lazy dog."

Each preview row should visibly use the corresponding local text style.

Order:

1\. display

2\. heading/h1

3\. heading/h2

4\. heading/h3

5\. heading/h4

6\. title/lg

7\. title/md

8\. body/lg

9\. body/md

10\. body/sm

11\. caption

12\. code/md

13\. code/sm

────────────────────────────────

PHASE 9 --- QUALITY CONTROL

────────────────────────────────

Before finishing, verify all of the following:

1\. All required collections exist and are local.

2\. All required modes exist with exact names.

3\. Semantic tokens alias primitives where expected.

4\. Light/dark semantic color values are meaningfully different where
needed.

5\. Radius modes switch correctly between default / rounded /
no-corner-radius.

6\. Variable scopes are applied intentionally and narrowly.

7\. WEB code syntax metadata exists where supported.

8\. All requested local text styles exist.

9\. Text styles are bound to semantic typography variables, not just
manually matched.

10\. Typography preview frame exists and uses the actual local text
styles.

11\. No component-specific tokens were created.

12\. Naming is clean and slash-based throughout.

After finishing, give a short summary of:

\- chosen brand color

\- chosen accent color or none

\- chosen neutral family

\- chosen heading/body font

\- whether all collections, modes, scopes, text styles, and preview
frame were created successfully

\- any fallback used

Important instruction:

Create variables first, then semantic aliases, then text styles, then
preview frame, and only after that run verification.
