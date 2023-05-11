# Build the manager binary
FROM golang:1.17 as builder

WORKDIR /workspace
# Copy the Go Modules manifests
COPY go.mod go.mod
COPY go.sum go.sum
# cache deps before building and copying source so that we don't need to re-download as much
# and so that source changes don't invalidate our downloaded layer
RUN go mod download

# Copy the go source
COPY main.go main.go
COPY api/ api/
COPY controllers/ controllers/

# Build
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -a -o manager main.go

# Use distroless as minimal base image to package the manager binary
# Refer to https://github.com/GoogleContainerTools/distroless for more details
FROM gcr.io/distroless/static:nonroot
WORKDIR /
COPY --from=builder /workspace/manager .
USER 65532:65532

ENTRYPOINT ["/manager"]
2023-05-11 15:57:12.745 [info] Log level: Info
2023-05-11 15:57:12.745 [info] Validating found git in: "C:\Program Files\Git\cmd\git.exe"
2023-05-11 15:57:12.745 [info] Using git "2.40.1.windows.1" from "C:\Program Files\Git\cmd\git.exe"
2023-05-11 15:58:25.377 [info] > git rev-parse --show-toplevel [664ms]
2023-05-11 15:58:27.854 [info] > git rev-parse --show-toplevel [962ms]
2023-05-11 15:58:41.333 [info] > git rev-parse --show-toplevel [107ms]
2023-05-11 15:58:41.424 [info] > git rev-parse --git-dir --git-common-dir [84ms]
2023-05-11 15:58:41.440 [info] Open repository: c:\Users\pc
2023-05-11 15:58:41.723 [info] > git config --get commit.template [262ms]
2023-05-11 15:58:41.742 [info] > git for-each-ref --format=%(refname)%00%(upstream:short)%00%(objectname)%00%(upstream:track)%00%(upstream:remotename)%00%(upstream:remoteref) refs/heads/main refs/remotes/main [249ms]
2023-05-11 15:58:41.764 [info] > git log --format=%H%n%aN%n%aE%n%at%n%ct%n%P%n%D%n%B -z -n120 -- c:\Users\pc\AppData\Roaming\Code\Workspaces\1682882432833\workspace.json [256ms]
2023-05-11 15:58:41.764 [info] fatal: your current branch 'main' does not have any commits yet
2023-05-11 15:58:42.140 [info] > git check-ignore -v -z --stdin [130ms]
2023-05-11 15:58:42.208 [info] > git show --textconv :AppData/Roaming/Code/Workspaces/1682882432833/workspace.json [131ms]
2023-05-11 15:58:42.224 [info] > git ls-files --stage -- C:\Users\pc\AppData\Roaming\Code\Workspaces\1682882432833\workspace.json [131ms]
2023-05-11 15:58:43.198 [info] > git ls-files --stage -- C:\Users\pc\AppData\Roaming\Code\Workspaces\1682882432833\workspace.json [85ms]
2023-05-11 15:58:43.420 [info] > git show --textconv :AppData/Roaming/Code/Workspaces/1682882432833/workspace.json [153ms]
2023-05-11 15:58:55.496 [info] > git status -z -uall [13744ms] (cancelled)
2023-05-11 15:59:03.487 [info] > git config --get commit.template [357ms]
2023-05-11 15:59:03.493 [info] > git for-each-ref --format=%(refname)%00%(upstream:short)%00%(objectname)%00%(upstream:track)%00%(upstream:remotename)%00%(upstream:remoteref) refs/heads/main refs/remotes/main [230ms]
2023-05-11 15:59:07.412 [info] > git status -z -uall [3911ms] (cancelled)
2023-05-11 15:59:36.033 [info] > git check-ignore -v -z --stdin [102ms]
2023-05-11 16:00:53.218 [info] > git check-ignore -v -z --stdin [113ms]

