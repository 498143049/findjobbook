int get_sum1(int n){
    int cc1=0;
    while(n){
        n&=n-1;
        cc1++;
    }
    return cc1;
}

nt get_sum(int n){
    int cc=0;
    if(n<10)
        return n;
    while(1){
        cc+=n-10*(int)(n/10);
        n=n/10;
        if(n==0)
            break;
    }
    return cc;
}