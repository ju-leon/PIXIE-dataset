# PIXIE Assembly Dataset

This dataset contains RGB images of LEGO models in different assembly states, including correct assemblies and defective (partially disassembled or mistaken) assemblies.

it contains test-only data and is intended for zero-shot methods.

For each model, the dataset provides:
- camera calibration parameters,
- a 3D model of the fully assembled target object,
- per-image 6DoF ground-truth poses,
- ground truth assembly-state labels.

Important: This is a test-only dataset designed for zero-shot evaluation.

## Task Definition

Goal: Estimate the 6DoF pose of the object in each image.

Protocol constraints:
- At inference time, methods must use only the fully assembled target model.
- Defective-state models must not be used for inference.
- Defective-state models and labels may be used only for evaluation and analysis.

This protocol evaluates whether a method can generalize from the canonical (fully assembled) object model to images where the visible object may be assembled incorrectly, partially disassembled, cluttered, or color-varied.

## Dataset Content

At repository level:
- [dataset/](dataset)

Each model folder (for example [dataset/model1/](dataset/model1/), [dataset/model2/](dataset/model2/)) contains:
- `<model_name>_poses.json`: metadata and pose-related annotations.
- `calibration/camera_calibration.json`: camera intrinsics (and additional calibration data if provided).
- `data/`: input images organized by scenario.
- `gt/`: ground-truth annotations and model-state references organized by scenario.
- (in some models) material/mesh files for the target model.

Common scenario groups include:
- `color*`: same geometry, different object colors.
- `clutter*`: cluttered scenes.
- `assembly_mistake*`: incorrect assembly states.
- `assembly_mistake_with_clutter*`: incorrect assembly combined with clutter.

Naming may vary slightly between models (for example `assembly_mistake_1` vs `assembly_mistake1`).

## Ground Truth

The dataset provides ground truth for:
- 6DoF object pose per image.
- Assembly state per image/scenario.

Use these labels only for evaluation, never as direct inference input.

## Zero-Shot Evaluation Rules

To ensure comparable results:
- Do not train or tune on this dataset.
- Do not use defective-state geometry at inference.
- Do not use ground-truth pose or assembly labels at inference.
- Report results separately by scenario type (recommended): color, clutter, assembly mistake, assembly mistake with clutter.

## Suggested Evaluation Workflow

1. For each model, load the fully assembled target geometry.
2. For each image in data/, run your method and predict object pose.
3. Match predictions with corresponding entries in gt/.
4. Compute pose metrics (for example rotation and translation error).
5. Optionally compute assembly/shape agreement metrics for analysis.
6. Aggregate results per scenario and overall.

Evaluation scripts and metric implementations are intentionally not included in this repository.

## Reporting Recommendations

When publishing results, include:
- method type (template-based, learned, foundation-model-based, etc.),
- whether any external training data was used,
- per-scenario performance,
- failure cases on defective assemblies,
- runtime and hardware details.

## Intended Use

This dataset is intended for benchmarking robust, zero-shot object pose estimation under:
- assembly defects,
- appearance variation (color),
- scene clutter,
- combined distribution shifts.

## Project Website (GitHub Pages)

A small static project page is available in [docs/](docs).

To publish it with GitHub Pages:
1. Go to repository Settings -> Pages.
2. Under Build and deployment, select Source: Deploy from a branch.
3. Select Branch: main (or your default branch) and Folder: /docs.
4. Save. GitHub will publish the site and provide the URL.

The page entry point is [docs/index.html](docs/index.html).

## License

This dataset is licensed under the Creative Commons Attribution 4.0 International
License (CC BY 4.0). See [LICENSE.md](LICENSE.md) for details.

The dataset is provided "as is", without warranties or conditions of any kind.
The authors and contributors disclaim liability for any use of the dataset.