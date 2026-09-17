# ImageGen 2.5: A Practical Guide

A working guide to GPT Image 2.5 Flare and Sunburst: choosing a model, controlling quality and cost, writing better prompts, making precise edits, and building image-powered products.

**Guide version:** September 18, 2026.

> **Working principle:** define the visual result, choose the right model and settings, provide useful references, make focused changes, and inspect the result before building on it.

## Contents

1. [Choose between Flare and Sunburst](#1-choose-between-flare-and-sunburst)
2. [Understand the three main improvements](#2-understand-the-three-main-improvements)
3. [Separate quality from resolution](#3-separate-quality-from-resolution)
4. [Estimate generation and editing costs](#4-estimate-generation-and-editing-costs)
5. [Choose dimensions and transparency](#5-choose-dimensions-and-transparency)
6. [Prompt for infographics and photorealism](#6-prompt-for-infographics-and-photorealism)
7. [Make focused edits and repeated refinements](#7-make-focused-edits-and-repeated-refinements)
8. [Build a virtual fitting room](#8-build-a-virtual-fitting-room)
9. [Create animation through sequential image edits](#9-create-animation-through-sequential-image-edits)
10. [Translate the workflow into API settings](#10-translate-the-workflow-into-api-settings)
11. [Create brand-consistent marketing assets](#11-create-brand-consistent-marketing-assets)
12. [Use images to guide web and product development](#12-use-images-to-guide-web-and-product-development)
13. [Choose between ChatGPT and API control](#13-choose-between-chatgpt-and-api-control)
14. [Review outputs and troubleshoot](#14-review-outputs-and-troubleshoot)

## 1. Choose between Flare and Sunburst

GPT Image 2.5 offers two models with different priorities. Both support generation and editing; the choice is primarily about speed versus demanding fidelity and editing requirements, rather than different token prices.

| Model | Main priority | Useful starting point |
| --- | --- | --- |
| GPT Image 2.5 Flare | Low latency with a quality baseline comparable to GPT Image 2 | Interactive previews, rapid iteration, and workflows that already produce acceptable results with GPT Image 2 |
| GPT Image 2.5 Sunburst | Higher fidelity and precise editing | Detailed creative work, changes that must preserve surrounding content, and demanding multi-step refinement |

Flare's latency improvement can be **up to 50%** relative to GPT Image 2 at comparable quality and resolution settings. Treat that as an upper-bound improvement, not a guarantee that every request will finish twice as quickly.

Sunburst is the natural candidate when subtle details matter: lifelike people, natural lighting, subject fidelity, or keeping an existing composition stable during edits. Flare can still produce strong text-to-image results; speed optimization does not automatically make it unsuitable for final assets.

**Practical selection process:** start with Flare for an interactive experience and Sunburst for a precision-sensitive one. Compare the same prompt and reference images. Judge the actual result at its intended display size, and retain the faster option when it meets the visual requirements.

For the API model names and supported settings, see OpenAI's [model-selection and prompting guide](https://developers.openai.com/api/docs/guides/image-prompting).

## 2. Understand the three main improvements

### More lifelike images and stronger fidelity

The areas to examine include recognizable subjects, lifelike people, and natural lighting, especially in outdoor scenes. Sunburst places particular emphasis on fidelity.

When evaluating a person or product image, do not stop at whether the overall composition looks attractive. Check the details that make the subject recognizable and the lighting that makes it belong in the scene.

### More focused edits

A successful local edit changes the requested element while keeping unrelated elements stable.

Consider a teapot on a table. Changing its lid to red is only part of the task. The table should not move upward, the teapot should not change size, and a small square in the background should not become larger or smaller.

This suggests a useful review pattern:

```text
Requested change: the teapot lid becomes red.
Preserve: teapot body, table height, object positions, background shapes,
lighting, framing, and image dimensions.
```

### Better repeated refinement

Successive edits can accumulate unwanted artifacts or gradually shift the composition. GPT Image 2.5 improves this behavior, making repeated refinement more practical.

Inspect quiet areas such as a wall, tabletop, or subtle surface imprint. They can reveal cumulative changes that are easy to miss when attention stays on the edited subject.

Improved preservation is not a reason to skip review. The useful outcome is an editing loop in which each accepted image remains a sound basis for the next change.

## 3. Separate quality from resolution

**Resolution determines the number and arrangement of pixels. Quality affects the fidelity of the content within those pixels.** A large image at low quality is not equivalent to a large image at high quality.

At the same resolution, higher quality can improve small regions that become noticeable when someone zooms in: dense lettering, fine textures, subtle surface details, and local image fidelity.

| Quality level | How to use it |
| --- | --- |
| Low | Fast previews, quick frame generation, and images where small detail is not the main requirement |
| Medium | An intermediate point for exploring the quality, price, and latency trade-off |
| High | A starting point for dense infographics and detail-sensitive assets |
| Extra high | Additional fidelity when high quality does not meet the requirement |
| Max | The top quality setting for demanding output |

For 2K and 4K images that will be examined closely, test higher settings rather than assuming resolution alone will supply the detail. For a quickly viewed preview or an image seen from a distance, low quality may be sufficient.

### Migration detail: the quality labels have changed meaning

GPT Image 2 exposed low, medium, and high. GPT Image 2.5 adds extra high and max, with intermediate points along the output-token curve.

The important comparison is that **GPT Image 2 high corresponds to GPT Image 2.5 max in output-token allocation**, rather than a simple high-to-high mapping. The expanded scale provides more control between the endpoints.

Consequently, migrating an application involves more than changing the model name. Compare the visual result and token consumption for the selected model, quality, and size. Matching a quality label across generations does not by itself establish matching cost or visual fidelity.

API spelling is covered in [section 10](#10-translate-the-workflow-into-api-settings); extra high is written as `xhigh` in an API request.

## 4. Estimate generation and editing costs

The standard token rates below are shared by GPT Image 2, Flare, and Sunburst. These are US-dollar rates, not an all-inclusive fixed price per image.

| Token category | Price per 1 million tokens |
| --- | ---: |
| Text input | $5 |
| Image input | $8 |
| Image output | $30 |

The Flare and Sunburst rates can be checked on their respective [Flare model page](https://developers.openai.com/api/docs/models/gpt-image-2.5-flare) and [Sunburst model page](https://developers.openai.com/api/docs/models/gpt-image-2.5-sunburst).

A standard-rate estimate is:

```text
request cost =
    text input tokens  × $5  / 1,000,000
  + image input tokens × $8  / 1,000,000
  + image output tokens × $30 / 1,000,000
```

Quality and output resolution affect output-token usage. Editing also consumes image input tokens, including the reference images supplied to the request.

### A useful small-image budgeting example

Use **approximately $0.006, or 0.6 cents, for a low-quality square 1K image** as an illustrative output-cost reference, not a universal per-request quote.

At that assumed output cost, 100 images would be $0.60 and 1,000 images would be $6.00 for output alone. Those are arithmetic examples: text input, reference-image input, and any separate planning-model calls are additional.

For an exact configuration, use a calculator that supports the chosen model and check request usage. Do not assume a GPT Image 2 estimate gives GPT Image 2.5 token consumption merely because the token rates are equal. The [image-generation guide](https://developers.openai.com/api/docs/guides/image-generation) includes pricing guidance.

### Animation multiplies the work

A multi-frame animation is a sequence of image requests. Budget for each generated frame and for the original and recent-frame references supplied repeatedly. Account separately for the text model that plans the motion.

Reducing quality for a preview can make iteration faster. Whether that setting is suitable for the final asset depends on the details and stability required by the finished animation.

## 5. Choose dimensions and transparency

### Match the aspect ratio to the destination

GPT Image 2 and 2.5 expand beyond the three output sizes of earlier image-model generations. The available combinations number in the tens of thousands, with roughly 30,000 resolution combinations and aspect ratios reaching approximately **3:1** in landscape or **1:3** in portrait.

This makes wide social banners and tall poster-style compositions practical without forcing every design into the same small set of shapes. Support extends up to 4K output.

Treat aspect ratio, resolution, and quality as separate decisions. A wide shape does not necessarily require the largest pixel budget, and the largest pixel budget does not automatically give the best small-detail rendering.

### Use transparency for reusable assets

Both Flare and Sunburst support transparent backgrounds. Useful applications include isolated products, clothing assets, design elements, and graphics that will be placed over different website backgrounds.

A transparent clothing asset can also improve a shopping interface: the item can be displayed as a clean silhouette and enlarged on hover without a rectangular background around it.

For a rectangular animation with a full scene, an opaque background may be the more appropriate choice. Transparency is an output requirement, not a universal quality improvement.

**API implementation note, checked September 18, 2026:** custom sizes must follow the API's dimension constraints; do not interpret flexible sizing as unrestricted sizing. The image-edit reference specifies dimensions divisible by 16, aspect ratios between 1:3 and 3:1, and marks resolutions above 2560×1440 as experimental. Transparent output requires PNG or WebP. Check the [image-edit API reference](https://developers.openai.com/api/reference/resources/images/methods/edit/) before fixing production dimensions.

## 6. Prompt for infographics and photorealism

The following templates are reusable examples. Replace bracketed fields with the actual content and requirements.

### High-density infographics: specify content and spatial layout

An infographic prompt can be open-ended, letting the model choose the layout, or highly prescriptive about where text and pictures belong.

Use the prescriptive approach when a deliverable must contain exact wording or follow a defined structure. Describe the image region by region rather than relying on a vague request for a polished infographic.

```text
Create an infographic explaining [process] for [audience].

Layout:
- Top: title using exactly this wording: "[title]".
- Left: a labeled illustration of [main object].
- Right: [number] numbered steps in reading order.
- Bottom: a compact summary using the approved text below.

Approved text:
[Insert the exact labels, step descriptions, and summary.]

Keep the hierarchy clear and the text readable.
Use illustrations and labels together to explain the process.
```

For example, an explanation of how an espresso machine works can combine a central machine illustration with a step-by-step explanation. The same structure works for a technical process, product explainer, or presentation graphic.

For dense text, begin with **high, extra high, or max quality**. Inspect the actual labels and their placement, not just the composition.

### Photorealism: use the explicit keyword

When an image looks more animated or illustrated than intended, include **photorealistic** explicitly in the prompt. Do not rely only on words such as beautiful or professional to communicate a photographic result.

```text
Create a photorealistic image of [subject] in [setting].

Use natural-looking lighting appropriate to the setting.
The subject should look lifelike rather than illustrated or animated.
Preserve the recognizable details in the supplied reference image.

Composition: [framing and placement].
Intended use: [product page, campaign image, or other destination].
```

The [image-prompting guide](https://developers.openai.com/api/docs/guides/image-prompting) provides additional examples of photographic prompting and information graphics.

### Visual references communicate taste more effectively

A reference design can communicate relationships between typography, imagery, spacing, and composition that are difficult to describe from scratch.

Use a text-capable agent to turn the reference into a design brief before generating the finished asset:

```text
Analyze the supplied design reference.
Describe its layout, typography, visual hierarchy, image treatment,
spacing, and overall look and feel.

Turn those observations into a design brief for [new deliverable].
Use our approved brand assets and content.
Show the brief before generating the final images.
```

This makes the intended direction inspectable instead of hiding it inside a broad request to make something look good.

## 7. Make focused edits and repeated refinements

### State both the change and the invariants

An edit prompt should describe what changes and what remains fixed. The preservation requirements are especially important when a result will be compared against a product reference or reused in later frames.

```text
Edit the supplied image.

Change only the teapot lid to red.

Keep the teapot body, table height, camera framing, lighting,
background shapes, and all other object positions unchanged.
Preserve the original image dimensions.
```

For a person, the invariants may include recognizable identity, outfit details, framing, and surrounding scene. For a product, they may include silhouette, label placement, proportions, and material appearance.

### Keep the refinement loop inspectable

```text
Original image
  → focused edit
  → inspect the requested change and preserved details
  → accept the result
  → next focused edit
```

When checking preservation, look beyond the main subject. A shifted table edge, resized background object, or changed wall texture can indicate that the edit affected more than requested.

A practical way to organize feedback is:

```text
Target region: [the exact area to edit].
Requested change: [one specific correction].
Keep unchanged: [elements and details that must remain stable].
Acceptance check: [what should look different and what should not].
```

For fine adjustments, canvas-style comments that identify a precise area can be more useful than asking the model to improve the entire image.

## 8. Build a virtual fitting room

A virtual fitting room combines a person's photo with multiple clothing reference images to create a personalized outfit preview.

### Inputs and output

| Input | Role |
| --- | --- |
| Person photo | Establishes the person to preserve |
| Selected garment images | Establish the clothing to combine into one outfit |
| Outfit instructions | Explain how the references should be used together |
| Output settings | Keep the preview dimensions and quality intentional |

The image-edit request receives multiple references, rather than relying on text descriptions of every garment.

```text
Person photo + selected shirt + selected jeans + selected jacket
  → image edit
  → outfit preview on the person
```

For a prototype, generated transparent clothing assets can make the interface easy to explore. For an actual store, use photographs of the real products so the references represent the items being offered.

### Reusable outfit prompt

```text
Create a photorealistic virtual try-on preview.

Use the first image as the person reference.
Use the remaining images as the selected clothing references.
Combine the selected items into one outfit worn by that person.

Preserve the person's recognizable appearance.
Keep the clothing faithful to the supplied product references.
Do not substitute different items or add unrelated garments.
```

### From preview to an interactive experience

Present the clothing choices, let the user select a combination, generate the outfit preview, and allow focused changes. The accepted preview can then become the starting frame for an animated turntable.

The useful architectural split is between **outfit composition** and **animation**. First establish the person wearing the intended items. Then animate that accepted result instead of solving clothing selection and motion independently in every frame.

An image preview shows an appearance concept; it does not by itself establish garment sizing or physical fit accuracy.

## 9. Create animation through sequential image edits

This workflow creates motion by generating a sequence of still images and assembling them into an animation. It is not a single image request that directly returns a video.

### The core loop

```text
Original photo + accepted outfit preview
  → text model creates a motion plan
  → image edit generates the next frame
  → repeat using stable references and the latest frame
  → export the completed sequence
```

A text model supplies the plan: what the subject should do and what should change from one frame to the next. The image model renders each step.

### Give each reference a distinct role

| Reference | Purpose |
| --- | --- |
| Original person photo | Anchors recognizable identity |
| First accepted outfit frame | Anchors the clothing and initial appearance |
| Most recent frame | Provides continuity with the preceding step |
| Next-frame direction | Specifies the particular movement to make now |

Keep the original and initial outfit references available across the sequence. Using only the latest generated frame gives the next request less direct access to the starting appearance.

### Prompt structure for every frame

Combine a stable instruction prefix, the overall animation plan, and one changing frame direction.

```text
Create the next frame of this outfit turntable.

Reference roles:
- Original photo: preserve the person's identity.
- First outfit frame: preserve the selected clothing and appearance.
- Most recent frame: continue the motion from this point.

Overall plan:
[Insert the approved motion plan.]

Next frame:
[Insert the direction for this step only.]

Keep the person, clothing details, background, framing, and lighting
consistent except for the planned motion.
Return a single image at the fixed output dimensions.
```

### Conceptual implementation

The following is **pseudocode**, not a complete SDK implementation. The planner, image-edit adapter, frame storage, review, and media export are application functions to implement.

```text
original = load_person_photo()
first_frame = create_and_accept_outfit_preview(original, selected_clothes)
plan = create_motion_plan(first_frame)

frames = [first_frame]
current_frame = first_frame

for direction in plan.frame_directions:
    prompt = combine(fixed_instructions, plan.overview, direction)

    next_frame = edit_image(
        model = chosen_image_model,
        references = [original, first_frame, current_frame],
        prompt = prompt,
        size = fixed_dimensions,
        quality = chosen_quality,
        output_format = "jpeg",
        background = "opaque"
    )

    inspect_identity_outfit_and_scene(next_frame)
    frames.append(next_frame)
    current_frame = next_frame

export_animation(frames, format = chosen_media_format)
```

The inspection step represents an acceptance gate: do not continue from a rejected frame without correcting it. It is an implementation choice for the workflow, not a built-in image API operation.

### Settings that matter

Keep frame dimensions constant throughout the sequence. Generate one next-frame image per step. Low quality is useful when the priority is generating preview frames quickly; inspect whether it preserves the details needed for the finished animation.

JPEG with an opaque background is appropriate for a full rectangular scene. Assemble frames into the desired delivery format, such as a GIF-style animation, WebM, or MP4, using a separate export step.

Pay particular attention to identity, clothing consistency, and the background when comparing adjacent frames. Better individual still images are not enough if the person or outfit visibly changes during playback.

## 10. Translate the workflow into API settings

The following **API implementation details were checked against official documentation on September 18, 2026**. They provide concrete parameter spellings for the workflows above.

| Requirement | API setting |
| --- | --- |
| Flare | `model="gpt-image-2.5-flare"` |
| Sunburst | `model="gpt-image-2.5-sunburst"` |
| Fixed quality | `quality="low"`, `"medium"`, `"high"`, `"xhigh"`, or `"max"` |
| Model-selected quality | `quality="auto"` |
| Fixed dimensions | `size="WIDTHxHEIGHT"`, using a supported combination |
| Image editing | Supply the relevant images to the image-edit operation |
| Transparent asset | `background="transparent"` with PNG or WebP output |
| Full-scene frame | `background="opaque"`; JPEG is one output option |

See the [image-generation guide](https://developers.openai.com/api/docs/guides/image-generation) and [image-edit reference](https://developers.openai.com/api/reference/resources/images/methods/edit/) for request schemas.

For repeatable comparisons, record the model, prompt, reference images, quality, dimensions, background, and output format. Otherwise, a change in one setting can be mistaken for a difference between models.

For an animation, the next-frame instruction and current-frame reference should change as the sequence progresses; the dimensions and the intended visual identity should not.

## 11. Create brand-consistent marketing assets

Image generation becomes more useful for marketing when it is connected to research, brand guidance, copy planning, and approval rather than treated as a one-off prompt.

### Carousel workflow

```text
Review recent brand posts
  → identify the visual style
  → load brand guidance
  → plan the carousel and propose copy
  → obtain approval
  → generate images
  → refine dimensions, typography, and details
```

Store recurring brand requirements in a reusable skill: correct logos, font sizes, and visual standards. This avoids repeatedly reconstructing the brand from a vague description.

The goal is consistency without making every slide identical. A carousel can have visual diversity and photorealistic imagery while still following the same brand rules.

### Reusable campaign brief

```text
Create a product carousel about [topic] for [audience].

First review the supplied examples of our recent posts.
Apply the approved brand guidance, logos, and typography.

Before generating images:
- Propose the slide sequence.
- Draft the exact text for each slide.
- Describe the intended visual for each slide.
- Ask for approval of the plan.

After approval, create the images and keep them visually consistent.
Use the required dimensions and refine any specifically marked areas.
```

### The final production adjustments

The last stage often concerns exact dimensions, font sizes, and localized changes rather than a new creative direction. Give feedback at that level of specificity.

For example, identifying the text block that needs a smaller font is more actionable than asking for a cleaner design. Keep the approved composition and correct only the area that prevents the asset from being ready to use.

## 12. Use images to guide web and product development

### Design before coding

A useful web-development sequence is to provide a visual reference, generate several layout concepts, choose one, and only then build the page.

```text
Visual reference + product goal
  → a few layout options
  → selected design
  → website implementation
  → comparison against the approved design
```

This creates a concrete target for the coding agent. It also makes it possible to reject an unsuitable direction before implementation work is built around it.

```text
Use this visual reference to explore a landing page for [purpose].
Create a few distinct layouts before implementing the website.
Apply our approved content and brand requirements.
Wait for me to select a layout, then build the page to match it.
```

### Put image generation inside the product

Image generation can also be part of a web app's user experience rather than only a tool used while building it.

One example is a calming drawing app: the user draws a simple shape, the application combines that drawing with an aesthetic reference, and image generation transforms the input into a finished visual.

```text
User drawing + stylistic reference
  → image generation or editing
  → generated result displayed in the app
```

Here, the drawing supplies the user's creative input and the reference supplies the intended visual treatment. The website coordinates the workflow.

### Other useful applications

The same building blocks support advertising assets, social graphics, physical-product mockups, merchandise concepts, presentation diagrams, and assets for games, websites, or video projects.

Choose the workflow around the final deliverable. A campaign needs brand consistency; a product mockup needs fidelity to the concept; a diagram needs readable information; and an embedded creative feature needs a clear interaction between user input and the generated result.

## 13. Choose between ChatGPT and API control

For planning, distinguish the desktop image experience from an API workflow with explicit parameters.

| Control | Desktop-app planning baseline | API workflow |
| --- | --- | --- |
| Aspect ratio | Request the intended shape in the prompt | Specify dimensions to determine the ratio |
| Resolution | Approximately a 1K pixel budget, with the shape varying | Explicit settings, including higher-resolution output up to 4K within supported constraints |
| Image-model selection | Automatically selected for the task | Explicitly choose Flare or Sunburst |
| Quality | Managed through the app experience | Set the quality parameter directly |
| Repeated frame generation | Conversational editing | An application-controlled sequence of requests |

The desktop baseline is a planning assumption, not a permanent limit for every client or account. Check the active interface before relying on exact output dimensions or model-selection controls.

A 1K pixel budget does not mean every image must be a square with both sides exactly 1024 pixels. An aspect-ratio request changes the shape while operating within the available overall image budget.

When comparing an image made in the desktop app with one made through Codex using API calls, inspect the actual model and settings. The application name alone is not enough to establish that the generation conditions were identical.

Use explicit API parameters when exact model selection, output dimensions, quality, or programmatic iteration are central to the product.

## 14. Review outputs and troubleshoot

### Review checklist

Before treating an image as finished, inspect it in the context where it will be used.

- **Content:** the requested subject, exact wording, and intended layout are present.
- **Fidelity:** recognizable people, clothing, products, and approved brand elements remain correct.
- **Preservation:** focused edits have not moved or resized unrelated objects.
- **Delivery:** dimensions, aspect ratio, transparency, and format match the destination.
- **Continuity:** successive edits or frames do not accumulate visible drift or artifacts.

### Common problems and first adjustments

| Problem | First adjustment to try |
| --- | --- |
| The image looks illustrated instead of lifelike | Add `photorealistic` and describe the intended lighting and appearance |
| A dense infographic has weak text or detail | Make the layout and exact text explicit; test high, extra high, or max quality |
| A large image has disappointing detail when enlarged | Increase quality instead of changing only resolution |
| An edit shifts unrelated parts of the scene | Narrow the change and explicitly name the elements that must stay fixed |
| Repeated edits degrade the scene | Compare against the starting image and correct drift before continuing |
| Animated frames change the person or outfit | Reuse the original photo, accepted first frame, and most recent frame with clear roles |
| Marketing images do not feel like one brand | Supply visual examples and a reusable brand-guidance skill |
| A coded page misses the intended design | Approve a visual layout before implementation and compare against that target |
| A workload is slower or costlier than expected | Revisit model, quality, resolution, and the number of generation steps |

### A compact working brief

Use this before starting a new image workflow:

```text
Deliverable:
Audience and destination:
Chosen image model:
Output dimensions and aspect ratio:
Quality setting:
Background and output format:
Reference images and the role of each:
Exact text to render:
Allowed changes:
Details to preserve:
Approval point:
Final visual checks:
```

The central habit is to make the result and its constraints explicit. Choose the model for the job, inspect the details that matter, and use iteration to improve a stable image rather than repeatedly replacing the whole visual direction.
