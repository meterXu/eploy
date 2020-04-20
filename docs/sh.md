```bash

find ./ -name  "test"  -exec sed  -i 's#somehost:[^\"]*#somehost:192\.168\.12\.23333#g' {} \;

```