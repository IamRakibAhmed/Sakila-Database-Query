# Sakila-Database-Query
## Here are some exercise of Sakila Database Query given below:


#### a. Use subqueries to display all actors who appear in fewer than 5 films.

select first_name, last_name from actor where actor_id in (
    select actor_id from film_actor where film_id in (
        select film_id from film where (convert("title", decimal)) < 5
    )
);


#### b. Find the names and email addresses of all Canadian customers using inner join or subquery.

select first_name, last_name, email from customer where address_id in (
    select address_id from address where city_id in (
        select city_id from city where country_id in (
            select country_id from country where country = "Canada"
        )
    )
);


#### c. Find the number of copies of the film Hunchback Impossible in the inventory system.

select count(inventory_id) as "Number of Copies" from inventory where film_id in (
    select film_id from film where title = "Hunchback Impossible"
);


#### d. Display customer names and total amount paid by each customer using subquery or inner join. Arrange the queries in descending order of the customers’ last names.

select first_name, last_name, sum(payment.amount) as "Total Amount" from customer
inner join payment on customer.customer_id = payment.customer_id 
group by customer.first_name, customer.last_name 
order by customer.last_name


#### e. Write a query to display how much business, in dollars, each store brought in.

select store.store_id, SUM(amount) as 'Revenue' from payment 
inner join rental on payment.rental_id = rental.rental_id 
inner join inventory on inventory.inventory_id = rental.inventory_id
inner join store on store.store_id = inventory.store_id
group by store.store_id


#### f. Display the first and last names of all actors whose last names end in ‘son’. (e.g. Johnson, Ericson etc.)
Ans:
select first_name, last_name from actor where last_name like '%son'

#### g. Print all addresses in which the second address is not empty.
Ans:
select address, address2, district, city_id, postal_code from address where address2 is null

#### h. List all film categories which have more than 50 films. (Display the name of each category and the number of films each contains)
Ans:
select category.name, count(film_category.film_id) as TotalFilms from film_category
inner join category on category.category_id = film_category.category_id
group by film_category.category_id having TotalFilms > 50  
order by TotalFilms


#### i. Show in how many film categories the average difference between the film replacement cost and the rental rate larger than 15.
Ans:
select count(film_category.category_id) as 'Number of categories' from film_category
inner join film on film_category.film_id = film.film_id
having avg(film.replacement_cost - film.rental_rate) > 15

#### j. Display all films which are emotional (according to description) or contain ‘angels’ in their title.
Ans:
select title from film where description like '%emotional%' or title like '%angels%'
