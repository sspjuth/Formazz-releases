# Integrating Formazz

The host owns the data. Pass Python values directly when evaluating a report;
there is no need to serialize them to JSON first.

```python
from pathlib import Path
from formazz.validation import evaluate_report_text
from formazz.render import render_html_document, render_pdf_document

report_path = Path("reports/sales.report")
result = evaluate_report_text(
    report_path.read_text(encoding="utf-8"),
    report_path,
    bindings={"sales": [["Espresso", 2, 35], ["Coffee", 1, 25]]},
)
html = render_html_document(result.document, result.styles, title="Sales")
Path("sales.html").write_text(html, encoding="utf-8")
# Requires the pdf extra and a locally installed Chromium browser:
render_pdf_document(result.document, result.styles, "sales.pdf")
```

For example, `reports/sales.report` could contain:

```text
sales_report(items as (product, quantity, price)):
    rows
        title("Sales")
        for item in items:
            row
                body(item.product)
                body(f"{item.quantity}")

sales_report(sales)
```

A theme can be supplied as `branding_source=theme_path.read_text()` and
`branding_path=theme_path` to `evaluate_report_text`. Pass
`appearance="light"` or `appearance="dark"` as required. The built-in Standard
theme provides defaults when no theme is supplied.

For native Qt, install the `qt` extra and use `formazz.render.qt_flow.ReportView`.
Create a QApplication first, evaluate the report, then give its document and
styles to the view's `set_report`. The host chooses when to refresh after data
changes and which actions to register. Qt views are scrollable; PDF export uses
the document's page structure.

Keep output and temporary files separate from installed engine files. The
application decides where reports, themes and font assets live.
