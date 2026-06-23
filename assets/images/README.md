# Artwork images

These three JPEGs are the **full-resolution sources** for the exhibit:

| File              | Artwork                          |
|-------------------|----------------------------------|
| `starry-night.jpg`| The Starry Night — Van Gogh 1889 |
| `great-wave.jpg`  | The Great Wave — Hokusai 1831    |
| `the-scream.jpg`  | The Scream — Munch 1893          |

All three are public domain.

## How they're displayed

The exhibit does **not** load these files at runtime. To stay self-contained
and host-independent (the DC preview doesn't resolve relative asset paths), each
artwork is **embedded directly in `index.html` as a base64 data URI** on its
`DATA[i].img` field, rendered via `<img src="…">`.

The embedded copies are downscaled to ~1000px wide / JPEG q72 (~150–200 KB each)
— plenty for a low-opacity backdrop and small card thumbnails.

### Re-embedding after changing a source

If you swap an image here, regenerate its data URI and replace the matching
`img: 'data:image/jpeg;base64,…'` string in the HTML, e.g.:

```python
from PIL import Image; import base64, io
im = Image.open("assets/images/the-scream.jpg").convert("RGB")
im.thumbnail((1000, 4000))
buf = io.BytesIO(); im.save(buf, "JPEG", quality=72, optimize=True, progressive=True)
print("data:image/jpeg;base64," + base64.b64encode(buf.getvalue()).decode())
```

> Note: `<img src>` is used rather than CSS `background-image` on purpose — the
> runtime parses style strings by splitting on `;`, which would truncate a data
> URI (`data:image/jpeg;base64,…`).
