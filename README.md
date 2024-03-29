# pdf-annotation

This repository reproduces issues with the `github.com/unidoc/unipdf/v3` package.

## Flattening annotations

### Source code

- `pdf_form_flatten.go` was copied from [unipdf-examples](https://github.com/unidoc/unipdf-examples/blob/4366cd9d61e9d2b1d70fa2940c104b34f1d489ce/forms/pdf_form_flatten.go). This was edited to set the `allannots` argument to `true`:
    ```go
    -       err = pdfReader.FlattenFields(false, fieldAppearance)
    +       err = pdfReader.FlattenFields(true, fieldAppearance)
    ```

### Input PDFs

- `test.pdf` was created using Google Docs and downloading as a PDF document.
- `test_with_boxes.pdf` was created by editing `test.pdf` using macOS Preview.
  Two grey boxes were added to the document. These are rectangle annotations
  with borders and a fill colour.

### Output PDFs

- `flattened_test_with_boxes.pdf` was generated by running the commands shown below.

### Observed behaviour

Scenario:
```gherkin
Given I have downloaded this Go module on a Mac with Go 1.12
When I flatten test_with_boxes.pdf
And I open flattened_test_with_boxes.pdf in macOS Preview
Then I can select the grey boxes and edit them
```

Output:
```bash
$ go run pdf_form_flatten.go $PWD test_with_boxes.pdf
[DEBUG]  parser.go:754 Pdf version 1.5
[DEBUG]  parser.go:1149 starting with "141 0 obj
<<
/Type /"
[DEBUG]  parser.go:1008 Incompatibility: Index missing coverage of 1 object - appending one - May lead to problems
[DEBUG]  parser.go:1091 Updating object number for XRef table 138 -> 141
[DEBUG]  parser.go:1149 starting with "3 0 obj
<< /Type /XR"
[DEBUG]  parser.go:1091 Updating object number for XRef table 3 -> 3
[DEBUG]  parser.go:1149 starting with "1 0 obj
<< /Type /XR"
[DEBUG]  parser.go:1091 Updating object number for XRef table 1 -> 1
Unlicensed copy of unidoc
To get rid of the watermark - Please get a license on https://unidoc.io
Total 1 processed / 0 failures

$ open flattened_test_with_boxes.pdf
```

### Desired behaviour

In the flattened PDF the two grey boxes should not be editable.
