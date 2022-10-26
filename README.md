# metaflow-diffusion
This project is to show how to run AI-generated images using the Stable Diffusion model with Metaflow workflow at scale. 
## Install Dependencies
```
pip install poetry
poetry install
```

## Run the code
### Step 1 : Download the Stable Diffusion Huggingface model
- Ensure that you have signed the waiver for [CompVis/stable-diffusion-v-1-4](https://huggingface.co/CompVis/stable-diffusion-v-1-4-original) model on the Huggingface hub.
- Create a [Huggingface hub access token](https://huggingface.co/docs/hub/security-tokens)
- Run the below command after replacing `<myhuggingfacetoken>` with the Huggingface hub token created in the previous step. Run this command only once to download the model to the local machine. 
    ```sh
    HF_TOKEN=<myhuggingfacetoken> python model_download.py
    ```
### Step 2: Run Metaflow Flows

### ⭐ Generate Images from a Simple Prompt ⭐ 
**Source File** : [meta_diffusers_text.py](./meta_diffusers_text.py)


**Run Command** : 
```sh
python meta_diffusers_text.py run \
    --max-parallel 10 \
    --num-images 40 \
    --prompt "Autumn inside the mars dome, ornate, beautiful, atmosphere, vibe, mist, smoke, fire, chimney, rain, wet, pristine, puddles by stanley artgerm lau, greg rutkowski, thomas kindkade, alphonse mucha, loish, norman rockwell" \
    --prompt "alan turing by pablo piccasso" \
    --num-steps 50 \
    --seed 9332
```

**Options**:
```
Options:
  --model-path PATH         Path to the downloaded model on the local machine.
                            [default: ./models]

  --force-upload            Force upload the model from the local machine
                            [default: False]

  --s3-prefix TEXT          prefix of the path where models are stored in S3.
                            [default: models/diffusion-models/]

  --model-version TEXT      [default: stable-diffusion-v1-4]
  --batch-size INTEGER      controls the number of images to send to the GPU
                            per batch  [default: 4]

  --width INTEGER           width of the output image  [default: 512]
  --height INTEGER          Height of the output image  [default: 512]
  --num-steps INTEGER       Number of steps to run inference  [default: 60]
  --prompt TEXT             [default: mahatma gandhi, tone mapped, shiny,
                            intricate, cinematic lighting, highly detailed,
                            digital painting, artstation, concept art, smooth,
                            sharp focus, illustration]

  --num-images INTEGER      Number of images to create per prompt  [default:
                            10]

  --seed INTEGER            [default: 42]
  --max-parallel INTEGER    This parameter will limit the amount of
                            parallelisation we wish to do. Based on the value
                            set here, the foreach will fanout to that many
                            workers.  [default: 4]
```

**Running Locally** : To run this flow locally, ensure that you have installed the `requirements.txt` file and commented the `@batch` decorator in the [flow file](./meta_diffusers_text.py).


### ⭐ Generate Many Images with Diverse Styles ⭐
**Source File** : [meta_dynamic_prompts.py](./meta_dynamic_prompts.py)

**Run Command** : 
```sh
python meta_dynamic_prompts.py run \
    --num-images 4 \
    --subject "mahatma gandhi" \
    --subject "alan turing" \
    --subject "albert einstein" \
    --subject "steve jobs" \
    --styles "Pablo Picasso, banksy, artstation" \
    --num-steps 45 \
    --seed 6372
```

**Options**:
```
Options:
  --model-path PATH          Path to the downloaded model on the local
                             machine.  [default: ./models]

  --force-upload             Force upload the model from the local machine
                             [default: False]

  --s3-prefix TEXT           prefix of the path where models are stored in S3.
                             [default: models/diffusion-models/]

  --model-version TEXT       [default: stable-diffusion-v1-4]
  --batch-size INTEGER       controls the number of images to send to the GPU
                             per batch  [default: 4]

  --width INTEGER            width of the output image  [default: 512]
  --height INTEGER           Height of the output image  [default: 512]
  --num-steps INTEGER        Number of steps to run inference  [default: 60]
  --subject TEXT              The subject based on which images are generated
                             [default: Mahatma gandhi, dalai lama, alan
                             turing]

  --styles TEXT              Comma seperated list of styles  [default: van
                             gogh,salvador dali,warhol,Art Nouveau,Ansel
                             Adams,basquiat]

  --num-images INTEGER       Number of images to create per (prompt, style)
                             [default: 10]

  --images-per-card INTEGER  Maximum number of images to show per @card
                             [default: 10]

  --ui-url TEXT              Url to the Metaflow UI. If provided then an index
                             card is created for the `join_styles` @step

  --seed INTEGER             Seed to use for inference.  [default: 42]
```

**Running Locally** : To run this flow locally, ensure that you have installed the `requirements.txt` file and commented the `@batch` decorator in the [flow file](./meta_dynamic_prompts.py).