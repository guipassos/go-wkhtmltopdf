# Gowkhtmltopdf
This is a docker image with wkhtmltopdf application and a server written in Golang to convert html to pdf;

The API receives an html through json file and returns a pdf file

# Run API with docker image

```sh

$ docker run --restart=always -d -p 5010:5010 --name gowkhtmltopdf \
	guipassos/gowkhtmltopdf:latest

$ docker logs -f <id-container>

$ curl -X POST localhost:5010/v1/api/topdf -H "Content-Type: application/json" \
--data @table.html.json --output /tmp/meuteste.pdf

```

# Building in your local machine

```bash

$ docker build --no-cache -f DockerfileUbuntu --build-arg PORT=5010 \
	-t guipassos/gowkhtmltopdf:latest .

// -- or alpine

$ docker build --no-cache -f DockerfileAlpine --build-arg PORT=5010 \
	-t guipassos/gowkhtmltopdf:latest .

$ docker run --restart=always -d -p 5010:5010 --name gowkhtmltopdf \
	guipassos/gowkhtmltopdf:latest

// -- or

$ docker run -p 5010:5010 --name gowkhtmltopdf -e X_KEY=xxxxxx \
	guipassos/gohtmltopdf

$ docker logs -f <id-container>

$ curl -X POST localhost:5010/v1/api/topdf -H "Content-Type: application/json" \
	 -H "Authorization:Basic xxxxxx" \
	--data @table.html.json --output /tmp/mytest.pdf

```

# Generating a json file with your html before submitting to the API

The API receives a json file, the filename and the html you want to convert and returns the PDF file;

To generate the html, you can use the below application.

Available fields:

```json
{
	"name":"MyPDF",
	"html":"<base 64 of your html here>",
	"grayscale":false,
	"nocollate":false,
	"image_dpi":600,
	"image_quality":94,
	"page_size":"A4",
	"orientation":"Portrait",
	"dpi":600,
	"margin_bottom":2,
	"margin_top":2,
	"margin_left":2,
	"margin_right":2
}
```
This app will convert your html to json and required fields for PDF generation;

```sh

$ cd makehtmljson
$ go run main.go --file table.html

```

# Running the server without using docker

```sh

$ go run gowkhtmltopdf.go

$ curl -X POST localhost:5010/v1/api/topdf -H "Content-Type: application/json" \
	--data @table.html.json --output /tmp/mytest.pdf

```

