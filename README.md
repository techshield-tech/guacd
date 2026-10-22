# guacd

Build image Docker `guacd` từ source [apache/guacamole-server](https://github.com/apache/guacamole-server) (bản stable mới nhất), multi-arch **linux/amd64** + **linux/arm64**, push lên GHCR.

Dùng Dockerfile gốc của upstream, không chỉnh sửa source.

## Sử dụng

```bash
docker pull ghcr.io/techshield-tech/guacd:latest
docker run -d --name guacd -p 4822:4822 ghcr.io/techshield-tech/guacd:latest
```

```yaml
services:
  guacd:
    image: ghcr.io/techshield-tech/guacd:1.6.0
    ports:
      - "4822:4822"
    environment:
      GUACD_LOG_LEVEL: info
    restart: always
```

## Tags

| Tag | Ý nghĩa |
|---|---|
| `X.Y.Z` | Đúng phiên bản upstream, ví dụ `1.6.0` |
| `X.Y` | Bản mới nhất của nhánh `X.Y` |
| `latest` | Bản stable mới nhất |

Nên pin `X.Y.Z` khi deploy production.

## Cơ chế build

Workflow [`.github/workflows/build.yml`](.github/workflows/build.yml):

- **Hàng ngày (02:00 UTC)**: lấy tag stable mới nhất của upstream, nếu chưa có trên GHCR thì build.
- **Thủ công** (Actions → Build guacd → Run workflow): chọn `version` bất kỳ (kể cả `-RC`), tick `force` để build lại tag đã có.
- Mỗi kiến trúc build native trên runner riêng (`ubuntu-24.04` / `ubuntu-24.04-arm`), sau đó gộp thành một manifest list.
