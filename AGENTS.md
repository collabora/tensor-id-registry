# Instructions for Tensor ID Registry

## Repository Purpose

This is a framework-agnostic registry for tensor encodings used in neural network inference. It documents:
- **Tensors**: Individual tensor specifications (in `tensors/`)
- **Tensor Groups**: Collections of related tensors that work together (in `tensor-groups/`)
- **Registry**: Central index in `tensor-id-register.md` linking all entries

The registry helps different frameworks (GStreamer, PyTorch, ONNX, etc.) consistently interpret tensor outputs from neural networks.

## Adding New Entries

### Naming Convention
Tensor IDs follow this pattern: `{architecture}-{variant}-{type}-{function}`
- Architecture: first model that uses this encoding (e.g., `yolo-v8`, `ssd-mobilenet-v1`)
- Variant: `-variant-1`, `-variant-2` (for derivatives)
- Type: `-out` (output), `-in` (input), `-inout` (both)
- Function: optional suffix describing role in tensor-group (e.g., `-boxes`, `-detections`, `-scores`)

Example: `yolo-v8-segmentation-out-detections`

### Source Quality and Variant Coverage (Required)

When documenting a new model family, do not stop at a single repo release page.

1. **Use primary sources first**:
   - Original paper/preprint (required)
   - Original/canonical implementation repo (required)
   - Concrete model artifacts (ONNX/TFLite/etc.) used to infer tensor shapes
2. **Add secondary implementations** when available (ports, SDK/toolkit decoders, re-implementations) to cross-check decoding behavior.
3. **Document major variants explicitly** (at minimum):
   - landmark/class count variants (for example 68/98/19),
   - dataset/supervision variants,
   - backbone variants,
   - export/runtime variants that change tensor shape/order.
4. **Earliest-origin requirement (tensors)**: when adding a new tensor, identify the earliest known model family that used that tensor encoding and name the tensor after that original source (not a later re-export/rebrand) when possible.
5. **Earliest-origin requirement (tensor-groups)**: when adding a tensor-group, identify and cite the earliest known source model/paper that defines that multi-tensor output contract.
6. **Reference-tracing requirement**: do not stop at one paper/repo. Follow references/citations backward ("climb up" the paper trail) to find the original encoding source, then cite both original and current practical implementation sources.
7. **Tensor reuse rule**: if a tensor encoding is equivalent to an existing/original encoding already in this registry, reuse the original tensor ID instead of creating a model-specific duplicate, and cite the original paper/source for that encoding.
8. **Tensor-group requirement**: when outputs are multi-tensor, the tensor-group file must explain how all tensors are decoded together (joint interpretation), not only list them.

### Tensor File Structure
Each tensor file (`tensors/{tensor-id}.md`) must include:
1. **Classification** section:
   - `tensor-group: yes/no`
   - `layer-type: input/output/inout`
   - `use-case: classification/object-detection/segmentation/etc.`
   - `part-of-tensor-groups: [list if applicable]`

2. **Description** section with:
   - Markdown table: `tensor-shape`, `tensor-datatype`, `tensor-id`
   - **Known Aliases** subsection
   - **Encoding** subsection with memory layout (Markdown table with Index/Value columns)

3. **External References**:
   - Link to original paper (required)
   - Code implementation link (required)

4. **Models**:
   - Link to reference model

5. **Tensor Decoders**:
   - Table with Framework/Links columns

### Tensor Group File Structure
Each tensor group file (`tensor-groups/{group-id}.md`) includes:
1. **Classification** section (mark `tensor-group: yes`)
2. **Description** with relationship table showing component tensors
3. **Tensor Decoding Logic**: pseudocode showing how components work together
4. **External References**, **Models**, **Tensor Decoders** sections
5. **Links to component tensors** at the end using markdown link references

Example link pattern at end of file:
```markdown
[count]: /tensors/generic-variant-1-out-count.md
[boxes]: /tensors/component-tensor.md
```

## Markdown Conventions

- Use standard Markdown tables with single `|` delimiters (not `||` at start/end)
- Table format: `|Column|Column|` with `|---|---|` separator
- Links to other tensors: use relative paths `/tensors/` or `/tensor-groups/`
- Memory layout tables: 2-column tables with "Index" and "Value" headers
- Memory layout index values must be **absolute flat offsets starting from 0**, expressed using symbolic dimension names (e.g. `NUM_CANDIDATES × 1 - 1`). Never use loop-variable expressions like `i×6 + 0`.

## Common Workflow

1. **Documenting a new model output**: Create a new file in `tensors/`
2. **Grouping related tensors**: Create a tensor-group file if multiple tensors form a model's complete output
3. **Updating registry**: Add entry to `tensor-id-register.md`
4. **Cross-referencing**: Link tensor files in tensor-group descriptions and vice versa

## Submission Guidelines
See `submission-guideline.md` for full requirements including:
- Minimum of one reference paper
- Public code implementation link
- Complete enough description to interpret without external reference

## MCP Servers

This repository is configured to use the following MCP servers:

### git
Enables agents to inspect commit history, track contributions, and analyze changes. Useful for:
- Understanding who added specific tensor definitions
- Reviewing changes to tensor documentation
- Tracing evolution of tensor encodings

### web
Enables agents to fetch and validate external resources. Useful for:
- Verifying links to referenced papers and models
- Checking availability of public code implementations
- Validating framework decoder links in Tensor Decoders sections

These servers enhance agents ability to maintain documentation quality and consistency across the registry.
