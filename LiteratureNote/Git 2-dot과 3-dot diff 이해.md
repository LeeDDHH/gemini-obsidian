# Git 2-dot과 3-dot diff 이해

**Original URL**: https://zenn.dev/waruby/articles/a89f9ba30b28fd

**Summary**:
Git의 2-dot diff(`git diff A..B`)는 두 브랜치 A와 B 사이의 단순한 차이를 보여주며, "A에서 B를 만들려면 어떻게 해야 하는가"에 대한 답을 제공합니다. 반면, 3-dot diff(`git diff A...B`)는 두 브랜치의 공통 조상(merge-base)과 두 번째 브랜치 B 사이의 차이를 나타내며, "B를 A에 병합하면 A에 어떤 변화가 생기는가"를 보여줍니다. Bitbucket과 같은 일부 도구는 브랜치 비교 시 3-dot diff를 사용합니다. 2-dot은 브랜치 간의 직접적인 변경 사항을, 3-dot은 병합 시의 영향을 파악하는 데 유용합니다.

**Date Created**: 2026-01-15