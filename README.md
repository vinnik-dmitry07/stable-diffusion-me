# A cherry-picking repo

You can run the [application](https://huggingface.co/spaces/Arkan0ID/dreambooth-dmitry-thumbs-up) with `<Dmitry>` and `<thumbs-up>` styles.
It requires ~10 minutes to build.

![image](https://github.com/vinnik-dmitry07/stable-diffusion-me/assets/24797359/e442db3d-802c-41c8-83a0-ad84837fb8a5)

Conceptually my findings and ideas are the following:

If we pass the generated image through CLIP and compute cosine similarity with the "thumbs up" prompt, we receive only ~0.2
This essentially means that the autoencoder part of Stable Diffusion adds ~0.8 additional semantics.

This also gives us simple heuristics to filter irrelevant results by cosine similarity threshold, or we can generate several images and select top-1.
You can use it via the top-k parameter. It is like rejection-based sampling: you sample N, then take the top K most appropriate images.
This is the most obvious/naive way to handle outliers, though Stable Diffusion 2.1 produces pretty consistent results for "thumbs up."

An extension of this approach could be a Few-shot Clip-guided Diffusion. But omitted it because it is really slow.

Another way to tackle the problem is to use Textual Inversion to produce new concepts from the _existing knowledge_ of the model.
But as I found out later, a textual inversion token "\<thumbs-up\>" works worth than a simple phrase "thumbs up" on SD v2.1

To add custom characters (adding _new knowledge_), we need DreamBooth SD alignment.
Thus I used SD v2.1 + DreamBooth and sequentially trained two concepts <Dmitry> and <thumbs-up>.
Because my laptop has 8 Gb VRAM, I trained the model on A100 in Colab using a tweaked ShivamShrirao/DreamBooth_Stable_Diffusion.ipynb notebook.
