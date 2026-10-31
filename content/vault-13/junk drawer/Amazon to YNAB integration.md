https://github.com/zjr/AmazonSyncForYNAB

Getting this to run local non-containerized wasn't awesome. Trying to get it running inside a container is a big pain. A lot of trouble was had trying to deal with the lack of binaries for arm64 cpus. I did some of this: `docker buildx build --platform=linux/amd64 -t ytoasync:latest .`; and this: `docker run --platform linux/amd64 ytoasync`.

I might enjoy one day making a Go version of this. Looked up some possible packages for the browser driver:

1. https://pkg.go.dev/github.com/chromedp/chromedp#section-readme
2. https://pkg.go.dev/github.com/go-rod/rod
3. https://pkg.go.dev/github.com/playwright-community/playwright-go

