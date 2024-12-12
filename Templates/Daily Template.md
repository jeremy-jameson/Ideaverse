---  
tags: daily
---  
  
## What do I need to accomplish today?

- [ ]  

## Notes

-  
  
## What did I learn today?

- 

> [!note]- Files created today
>```dataview  
>LIST WHERE striptime(created) = date(this.file.name) SORT file.name
>```

> [!note]- Files modified today
>```dataview  
>LIST WHERE striptime(modified) = date(this.file.name) SORT file.name
>```