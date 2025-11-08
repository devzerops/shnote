- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 순차 삽입 

    push(S, x)
	top ← top + 1;
	if(top > stack_SIZE) then
		overflow;
	else
		S(top) ← x;
    end push()

## 순차 삭제

    pop(S)
	if(top = 0) then underflow;
	else {
			return s(top);
			top ← top - 1;
		 }
    end pop()

## 스텍의 삽입

    push(s, item)
            newnode ← getnode();
            newnode.data ← item;
            newnode.link ← top;
            top ← newnode;
    end push()


## 스텍의 삭제

    pop(s)
	if(top = null) then
		return null;
	else {
			item ← top.data;
			top = top.link;
			returnnode(oldnode);
			return item;
		 }
    end pop()

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
