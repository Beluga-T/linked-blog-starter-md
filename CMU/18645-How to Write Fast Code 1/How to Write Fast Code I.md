## What is Fast Code?
Given a particular machine, what the fastest implement you can do

## Optimization is not scalable
## Pipeling
- making as much `independent` instruction into the pipeline as possible.
- use as many pipelines as possible
- avoid stalling any of the pipelines.
## Benchmarking

### want to know determine machine capability
- `latency` of instruction
	- Min # cycle before next `dependent` instruction
- instruction throughput
	- instruction processed per clock cycle
- `key assumption`: instruction is fully pipelined

### Timing
- wall clcok time `gettimeofday`
- rdtsc (return time stamp counter). `_rdtsc`
``` c++
unsigned long long rdtsc() { 
	unsigned long long int x; 
	unsigned a, d; 
	__asm__ volatile("rdtsc" : "=a" (a), "=d" (d)); 
	return ((unsigned long long)a) | (((unsigned long long)d) << 32); 
}
```

