1. 换行
```python
def return_tex_template(width):
    with open(TEMPLATE_TEX_FILE, "r") as infile:
        PRE_JUSTIFY_TEXT = infile.read()
        JUSTIFY_TEXT = PRE_JUSTIFY_TEXT.replace(
        TEX_TEXT_TO_REPLACE,
        "\\begin{tabular}{p{%s cm}}"%width + TEX_TEXT_TO_REPLACE + "\\end{tabular}")
        return JUSTIFY_TEXT

    
class TextJustify(TexMobject):
    CONFIG = {
        "alignment": "\\justify",
        "arg_separator": "",
        "j_width":6
    }
    def __init__(self,tex_string, **kwargs):
        digest_config(self, kwargs)
        assert(isinstance(tex_string, str))
        self.tex_string = tex_string
        file_name = tex_to_svg_file(
            self.get_modified_expression(tex_string),
            return_tex_template(self.j_width)
        )
        SVGMobject.__init__(self, file_name=file_name, **kwargs)
        if self.height is None:
            self.scale(TEX_MOB_SCALE_FACTOR)
        if self.organize_left_to_right:
            self.organize_submobjects_left_to_right()


class JustifyScene(Scene):
    def construct(self):
        text = TextJustify("""
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
            """,j_width=4).scale(0.5)
        self.add(text)
```
